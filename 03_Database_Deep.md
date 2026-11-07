# 03. Database Deep: PostgreSQL, Prisma, Redis, pgvector

> Mobile developers ki sabse aam kamzori: SQL aur database design. Product companies ka interview yahan pakadta hai.
> Practice ke liye ek schema neeche diya hai (VERDE jaisa). Har query pehle khud likho, phir chalao, phir `EXPLAIN ANALYZE`.

---

## 1. Practice setup (1 ghanta)

```bash
docker run -d --name pg -e POSTGRES_PASSWORD=dev -p 5432:5432 postgres:16
docker exec -it pg psql -U postgres
```

**Practice schema (VERDE jaisa):**
```sql
CREATE TABLE organizations (
  id BIGSERIAL PRIMARY KEY,
  name TEXT NOT NULL,
  status TEXT NOT NULL DEFAULT 'active' CHECK (status IN ('active','suspended')),
  created_at TIMESTAMPTZ NOT NULL DEFAULT now()
);

CREATE TABLE users (
  id BIGSERIAL PRIMARY KEY,
  org_id BIGINT NOT NULL REFERENCES organizations(id),
  email TEXT NOT NULL UNIQUE,
  role TEXT NOT NULL,
  created_at TIMESTAMPTZ NOT NULL DEFAULT now()
);

CREATE TABLE projects (
  id BIGSERIAL PRIMARY KEY,
  org_id BIGINT NOT NULL REFERENCES organizations(id),
  name TEXT NOT NULL,
  typology TEXT NOT NULL,
  created_at TIMESTAMPTZ NOT NULL DEFAULT now()
);

CREATE TABLE documents (
  id BIGSERIAL PRIMARY KEY,
  project_id BIGINT NOT NULL REFERENCES projects(id) ON DELETE CASCADE,
  credit_code TEXT,                -- e.g. 'WEPR2'
  file_name TEXT NOT NULL,
  size_bytes BIGINT NOT NULL,
  sha256 TEXT NOT NULL,
  version INT NOT NULL DEFAULT 1,
  is_archived BOOLEAN NOT NULL DEFAULT false,
  uploaded_by BIGINT REFERENCES users(id),
  created_at TIMESTAMPTZ NOT NULL DEFAULT now()
);

CREATE TABLE analysis_runs (
  id BIGSERIAL PRIMARY KEY,
  project_id BIGINT NOT NULL REFERENCES projects(id),
  credit_code TEXT NOT NULL,
  status TEXT NOT NULL,            -- queued/running/completed/failed
  points_achieved INT,
  result JSONB,
  created_at TIMESTAMPTZ NOT NULL DEFAULT now()
);

CREATE TABLE wallets (
  org_id BIGINT PRIMARY KEY REFERENCES organizations(id),
  balance INT NOT NULL CHECK (balance >= 0),
  held INT NOT NULL DEFAULT 0 CHECK (held >= 0)
);
```

Fake data daalo (`generate_series` se 1 lakh documents):
```sql
INSERT INTO organizations(name) SELECT 'Org '||g FROM generate_series(1,50) g;
INSERT INTO projects(org_id,name,typology)
  SELECT (random()*49+1)::int, 'Project '||g, (ARRAY['NC','CS','Retail','Schools'])[1+floor(random()*4)]
  FROM generate_series(1,2000) g;
INSERT INTO documents(project_id,credit_code,file_name,size_bytes,sha256)
  SELECT (random()*1999+1)::int, (ARRAY['WEPR2','EACR1','IEQCR2','MRCR2','LTCR4'])[1+floor(random()*5)],
         'file_'||g||'.pdf', (random()*5e7)::bigint, md5(g::text)
  FROM generate_series(1,100000) g;
```

---

## 2. SQL: 30 exercises (easy → hard)

**Basics**
1. Ek project ke saare non-archived documents, naye pehle.
2. Har credit code mein kitne documents (`GROUP BY`).
3. Wo credits jinme 1000 se zyada documents (`HAVING`).
4. Har org ka total storage (MB) (`JOIN` + `SUM`).
5. Wo projects jinme ek bhi document nahi (`LEFT JOIN ... WHERE d.id IS NULL` aur `NOT EXISTS` dono).

**Intermediate**
6. Har project ka sabse bada document (window: `ROW_NUMBER() OVER (PARTITION BY project_id ORDER BY size_bytes DESC)`).
7. **Har project + credit ka latest analysis run** (VERDE ka asli sawaal; `DISTINCT ON` aur window function dono se).
8. Duplicate files: same `sha256` jo ek project mein 2+ baar hai.
9. Har mahine ke uploads ka count, pichhle mahine se % change (`date_trunc` + `LAG`).
10. Running total storage per org by date (`SUM() OVER (ORDER BY ...)`).
11. Top 3 credits per typology by document count (window + filter).
12. Wo users jinhone pichhle 30 din mein kuch upload nahi kiya.
13. CTE se: org → projects → documents ka summary ek query mein.
14. Recursive CTE: folder tree (khud ek `folders(id, parent_id, name)` table banao) ka poora path.
15. JSONB: `result->>'verdict' = 'compliant'` wale runs; `result->'gaps'` array ki length.

**Advanced**
16. Cursor pagination: `WHERE (created_at, id) < ($1, $2) ORDER BY created_at DESC, id DESC LIMIT 20`.
17. Upsert: `INSERT ... ON CONFLICT (project_id, file_name) DO UPDATE SET version = documents.version + 1`.
18. Wallet hold (neeche Section 4).
19. Gaps and islands: lagataar din jinme upload hua.
20. Percentile: p95 file size per credit (`percentile_cont`).

**Interview-style (likh kar)**
21. Second highest salary (classic).
22. Department-wise top 3 earners.
23. Users jo lagataar 3 din login hue.
24. Customers jinhone kabhi order nahi kiya.
25. Duplicate emails delete karo, sabse purana rakho.
26. Har user ka first aur last order.
27. Month-over-month revenue growth.
28. Friends of friends (self join).
29. Pivot: rows ko columns (`FILTER` clause / `CASE`).
30. Median without `percentile_cont`.

Practice ke liye: **LeetCode Database (SQL 50)**, DataLemur, StrataScratch.

---

## 3. Indexes (yahan log fail hote hain)

| Concept | Gehrai |
|---|---|
| B-tree | Default; `=`, `<`, `>`, `BETWEEN`, `ORDER BY`, prefix `LIKE 'abc%'` |
| **Composite index ka order** | `(project_id, credit_code)` → `WHERE project_id=?` ✅, `WHERE project_id=? AND credit_code=?` ✅, sirf `WHERE credit_code=?` ❌ (leftmost prefix rule) |
| Covering index | `INCLUDE (file_name)` → index-only scan |
| Partial index | `WHERE is_archived = false` → chhota, tez |
| GIN | JSONB, arrays, full-text search (`tsvector`) |
| Expression index | `ON lower(email)` |
| Kab index nuksaan | Har write slow, storage; low-cardinality column (boolean akela) |
| Unique index | Data integrity + speed |

**`EXPLAIN ANALYZE` padhna:** Seq Scan vs Index Scan vs Index Only Scan vs Bitmap Heap Scan; `rows` estimate vs actual; `cost`; Sort / Hash Join / Nested Loop kab.

**Lab:**
1. Exercise 7 (latest run per credit) bina index chalao, time note karo.
2. `CREATE INDEX ON analysis_runs (project_id, credit_code, created_at DESC);`
3. Dobara chalao, plan compare karo. Ye kahani interview mein sunao.

---

## 4. Transactions, isolation, locks

**ACID** ka har letter ek example ke saath.

**Isolation levels (Postgres):** Read Committed (default), Repeatable Read, Serializable. Anomalies: dirty read (Postgres mein nahi), non-repeatable read, phantom, **lost update**, write skew.

**Wallet hold → settle (VERDE ka asli pattern, project-service `walletService.js`):**
```sql
-- HOLD (job start)
BEGIN;
SELECT balance, held FROM wallets WHERE org_id = $1 FOR UPDATE;   -- row lock
-- app check: balance - held >= estimate, warna ROLLBACK + INSUFFICIENT_CREDITS
UPDATE wallets SET held = held + $2 WHERE org_id = $1;
INSERT INTO wallet_holds(id, org_id, job_id, amount, status) VALUES (gen_random_uuid(), $1, $3, $2, 'held');
COMMIT;

-- SETTLE (job end): actual kharcha kaato, bacha hold chhodo
BEGIN;
SELECT * FROM wallet_holds WHERE job_id = $1 AND status = 'held' FOR UPDATE;   -- idempotency
UPDATE wallets SET balance = balance - $actual, held = held - $holdAmount WHERE org_id = $2;
UPDATE wallet_holds SET status = 'settled' WHERE job_id = $1;
COMMIT;
```
Alternative bina explicit lock: atomic conditional update
```sql
UPDATE wallets SET held = held + $2
WHERE org_id = $1 AND balance - held >= $2
RETURNING *;   -- 0 rows = insufficient
```
**Sawaal jo poocha jaayega:** settle do baar chal gaya to? (status check = idempotent). Deadlock kab hota hai (do rows alag order mein lock), kaise bachoge (hamesha same order).

**Optimistic locking:** `version` column; `UPDATE ... WHERE id=? AND version=?`; 0 rows = conflict → retry/409. Mobile offline sync ke conflicts ke liye bhi yahi idea.

---

## 5. Schema design

| Topic | Kya |
|---|---|
| Normalization | 1NF, 2NF, 3NF ek example se; kab **denormalize** (read-heavy counters, `last_message_at`) |
| Relationships | 1:1, 1:N, M:N (join table: `document_credits(document_id, credit_code)`; VERDE abhi "multi-credit" ke liye **copies** banata hai, join table behtar hota) |
| Keys | BIGSERIAL vs UUID (v7 sortable), natural vs surrogate |
| Soft delete | `deleted_at`; unique constraints ke saath problem (partial unique index) |
| Audit | `created_at`, `updated_at`, `created_by`; audit log table |
| JSONB kab | Flexible/AI output (VERDE `result`), lekin jo field query/filter hoti hai use column banao |
| **Multi-tenancy** | Shared table + `org_id` (VERDE), schema-per-tenant, DB-per-tenant; trade-offs. **Row Level Security** (`CREATE POLICY ... USING (org_id = current_setting('app.org_id')::bigint)`) se IDOR DB level par rokna |
| Migrations | Expand → migrate → contract; badi table par index `CONCURRENTLY`; column rename kabhi ek step mein nahi |

**Design exercise:** "Document versions + multi-credit linking + annotations" ka schema banao jo VERDE ki 3 problems hal kare: (1) copies ki jagah links, (2) replace karne par purane version ke annotations, (3) affected analysis runs flag karna.

---

## 6. Prisma (VERDE ka ORM)

| Topic | Kya |
|---|---|
| Schema | `model`, `@relation`, `@@index`, `@@unique`, enums |
| Queries | `findMany` + `where` + `select`/`include`, `cursor` pagination, `upsert`, `createMany` |
| N+1 | Loop mein `findUnique` ❌ → `include` ya `where: { id: { in: ids } }` ✅ |
| Transactions | `$transaction([...])` (batch) vs interactive `$transaction(async (tx) => ...)` |
| Raw SQL | `$queryRaw` tagged template (parameterized, safe) vs `$queryRawUnsafe` (injection risk) |
| Migrations | `migrate dev` (local, shadow DB) vs `migrate deploy` (prod) |
| Limits | Complex reporting queries, window functions → raw SQL behtar |
| Connection pool | Serverless mein connection exhaustion; pool size |

**VERDE mein dekho:** `verde-monorepo-main/packages/database/prisma/schema.prisma` (59 models). Exercise: 5 sabse important models (Organization, User, Project, Document, segregation job) ka ER diagram khud banao, aur dekho kaunse indexes hain aur kaunse missing lagte hain.

---

## 7. Redis (gehrai)

| Data type | Use case |
|---|---|
| String | Cache, counters (`INCR`), locks (`SET key val NX PX 30000`) |
| Hash | Object fields (job progress: `HSET job:123 processed 12 total 20`) |
| List | Simple queues |
| Set | Unique items (online users) |
| Sorted set | Leaderboard, rate limit sliding window, delayed jobs (Bull isi par) |
| Stream | Event log, consumer groups |
| Pub/Sub | Fire-and-forget events (VERDE event-bus) |

Samjho: TTL aur eviction policies (`allkeys-lru`), persistence (RDB vs AOF), Redis single-threaded kyun tez, distributed lock ke khatre (lock expire ho gaya kaam chalte hue → fencing token), cluster/sentinel ka idea.

**Exercise:** Redis mein (1) sliding window rate limiter (sorted set), (2) "ek project ek job" lock with TTL, (3) job progress hash + pub/sub se UI update.

---

## 8. Vector search: pgvector

```sql
CREATE EXTENSION vector;
CREATE TABLE chunks (
  id BIGSERIAL PRIMARY KEY,
  document_id BIGINT REFERENCES documents(id) ON DELETE CASCADE,
  org_id BIGINT NOT NULL,
  page INT,
  content TEXT,
  embedding vector(1536)
);
CREATE INDEX ON chunks USING hnsw (embedding vector_cosine_ops);
CREATE INDEX ON chunks (org_id);

-- top 5 similar chunks, sirf apne org ke
SELECT id, document_id, page, content, 1 - (embedding <=> $1) AS similarity
FROM chunks
WHERE org_id = $2
ORDER BY embedding <=> $1
LIMIT 5;
```
Samjho: cosine vs L2 vs inner product; HNSW vs IVFFlat (speed/recall/build time); filter + vector search ki problem (pre-filter vs post-filter); **hybrid search** (`tsvector` full-text + vector, Reciprocal Rank Fusion). VERDE ChromaDB use karta hai; interview mein pgvector vs dedicated vector DB (Pinecone, Qdrant) ka trade-off bolo: ek kam system vs scale/features.

---

## 9. NoSQL ka idea (interview ke liye)

MongoDB (documents, flexible schema), DynamoDB (partition key + sort key design, access patterns pehle), Cassandra (write-heavy, wide-column). **SQL vs NoSQL** ka jawab: data relationships, consistency, query patterns, scale, team ki expertise. Mobile ke liye: Firebase Firestore (offline sync, security rules) bhi jaanna achha hai.

---

## 10. Database ke 25 interview sawaal

1. Index kya hai; B-tree kaise kaam karta hai (high level)?
2. Composite index aur leftmost prefix.
3. Index kab nahi lagana?
4. `EXPLAIN ANALYZE` mein seq scan dikha, kyun ho sakta hai jab index hai?
5. Clustered vs non-clustered index (MySQL vs Postgres).
6. ACID ek example ke saath.
7. Isolation levels aur lost update.
8. Pessimistic vs optimistic locking; kab kya.
9. Deadlock kya, kaise bachoge?
10. Do log ek saath last ticket book karein.
11. Normalization; kab denormalize.
12. N+1 problem aur fix.
13. Offset pagination slow kyun hoti hai badi tables par?
14. Soft delete ke saath unique email.
15. Multi-tenant SaaS ka schema; data leak kaise rokoge?
16. Zero-downtime migration (column rename).
17. JSONB kab, column kab?
18. Read replicas; replication lag ka asar.
19. Sharding vs partitioning.
20. Connection pooling kyun (PgBouncer)?
21. Redis cache vs DB: consistency kaise?
22. Distributed lock Redis se; khatre.
23. SQL vs NoSQL ek naye chat app ke liye.
24. Vector DB kya hai; pgvector kab kaafi hai?
25. Backup aur point-in-time recovery ka concept.
