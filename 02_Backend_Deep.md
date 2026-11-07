# 02. Backend Deep: Node.js + TypeScript (10 hafte, VERDE par practice)

> Goal: "mobile developer jo backend jaanta hai" nahi, balki **"jo khud ek production-grade API bana aur debug kar sake"**.
> Har hafte: concept → chhota khud ka code → VERDE ki asli file mein wahi cheez dhoondhna → ek exercise.
> VERDE paths `verde-monorepo-main/services/` se relative hain.

---

## Hafta 1: Node.js kaise kaam karta hai

**Concepts (gehrai):**
- **Event loop phases:** timers → pending callbacks → poll → check (`setImmediate`) → close; `process.nextTick` aur Promise microtasks har phase ke beech.
- Single thread + **libuv thread pool** (fs, crypto, dns, zlib; default 4 threads, `UV_THREADPOOL_SIZE`).
- CPU-heavy kaam event loop rokta hai → `worker_threads` ya alag service (VERDE ne PDF rendering ke liye **Go service** banayi, yahi wajah).
- Streams: readable, writable, transform, **backpressure**, `pipeline()`. Badi file ko memory mein load kiye bina upload/download.
- CommonJS vs ESM (VERDE backend CommonJS hai).
- Error handling: sync throw, rejected promise, `unhandledRejection`, `uncaughtException` (process restart karo, chalate mat raho).

**Khud banao:** ek script jo `console.log` order predict kare (`setTimeout`, `setImmediate`, `nextTick`, `Promise.then`). Pehle likh kar guess karo, phir chalao.

**VERDE mein dekho:** `document-service/src/server.js` aur `app.js`: server kaise start hota hai, graceful shutdown hai ya nahi.

**Interview sawaal:**
1. Node single-threaded hai to ek saath 10,000 requests kaise?
2. `nextTick` vs `setImmediate` vs `setTimeout(0)` ka order.
3. 2 GB file ko 200 MB RAM wale server par kaise process karoge?
4. Ek route mein heavy JSON parse se baaki requests slow kyun hui?

---

## Hafta 2: TypeScript backend ke liye + Express

**TypeScript:** strict mode, `unknown` + narrowing, discriminated unions (API result types), generics (`Repository<T>`), utility types, **zod se runtime validation + type inference** (`z.infer<typeof Schema>`), `tsconfig` basics, `tsx`/`ts-node` dev setup.

**Express gehrai se:**
- Middleware chain ka order; `next()` vs `next(err)`; error middleware ke 4 arguments.
- Async errors: Express 4 mein async handler ki rejection pakadti nahi (wrapper chahiye); Express 5 mein pakadti hai.
- Router structure: `routes → controller → service → repository`.
- `helmet`, `cors` (credentials ke saath origin `*` nahi), `compression`, body size limits.
- Request ID middleware (har log mein ID).

**Khud banao:** "Tasks API" TS + Express + zod: `GET/POST/PATCH/DELETE /tasks`, central error handler, ek jaisa error format:
```json
{ "error": { "code": "VALIDATION_FAILED", "message": "title is required", "details": [...], "requestId": "..." } }
```

**VERDE mein dekho:** `document-service/src/routes/segregation.js` → kaunsa middleware har route par lagta hai (auth, permission, validation)? `packages/errors` ka error format. Note karo ki har service ka format ek jaisa hai ya nahi (MVP review mein mila: **nahi hai**).

**Exercise:** `routes/segregation.js` ke 3 routes ka flow ek diagram mein: route → middleware → controller → service → DB/queue.

---

## Hafta 3: REST API design (SDE-2 level)

| Topic | Kya aana chahiye |
|---|---|
| Resources | `/projects/{id}/documents`, verbs nahi (`/getDocuments` galat) |
| Status codes | 200, 201 (+Location), 202 (async job accepted), 204, 400, 401, 403, 404, 409 (conflict/duplicate), 422, 429, 500, 503 |
| Pagination | Offset (`?page=&limit=`) vs **cursor** (`?cursor=&limit=`): cursor kyun behtar badi/badalti lists ke liye |
| Filtering / sorting | `?status=failed&sort=-createdAt` |
| **Idempotency** | `Idempotency-Key` header; payment/job-create jaise POST dobara aaye to same result |
| Async operations | `POST /jobs` → 202 + `jobId` → `GET /jobs/{id}` polling ya websocket/push |
| Versioning | `/v1/`, header, ya backward-compatible changes (field add karna safe, rename nahi) |
| Mobile-friendly | Chhote payloads, partial responses, batch endpoints, ETag/304 |
| Docs | OpenAPI spec |

**Khud banao:** Tasks API mein cursor pagination + idempotency key (Redis mein 24 h ke liye response store).

**VERDE mein dekho:** segregation job ka flow (create → upload files → start → status polling) asli async-job API ka example hai. `SegregationStatusBanner.tsx` (frontend) har 3 s poll karta hai.

**Interview sawaal:** "Payment API design karo jahan mobile app ka network beech mein toot jaata hai" (idempotency). "Badi export ke liye API" (202 + job).

---

## Hafta 4: Authentication aur Authorization

**Authentication:**
- Password: bcrypt/argon2, salt, kabhi plain/MD5 nahi.
- **JWT:** header.payload.signature; access token chhota (15 min), refresh token lamba; refresh token **rotation** + reuse detection; logout/revoke ke liye denylist ya token version.
- Sessions (server-side, Redis) vs JWT: trade-offs.
- OAuth2 / OIDC: Authorization Code + **PKCE** (mobile apps ke liye zaroori), ID token vs access token. VERDE: Azure AD (`passport-azure-ad`), Google Drive OAuth.

**Authorization:**
- RBAC (roles → permissions), ABAC (attributes), **resource ownership checks**.
- Multi-tenant: har query `organizationId` se scope.
- **IDOR (Insecure Direct Object Reference):** `GET /jobs/123` login hai lekin job kisi aur org ka. OWASP API Security ka #1.

**Khud banao:** Tasks API mein signup/login, access + refresh with rotation, `requirePermission('tasks:delete')` middleware, aur har query mein `userId` scope.

**VERDE exercise (asli bug):**
1. `auth-service` mein login → JWT kaise banta hai, kya claims hain (`organizationId`, permissions?).
2. `document-service/src/routes/segregation.js` ke `GET` job-status routes kholo: job DB se lene ke baad kya `job.organizationId === req.user.organizationId` check hota hai? MVP review ke hisaab se kuch routes mein nahi.
3. Fix design karo: ek `loadJobForOrg(jobId, orgId)` helper jo mismatch par **404** de (403 nahi, taaki existence leak na ho). Saare job routes isi se.
4. Test likho: org A ka user, org B ka jobId → 404.

**Interview sawaal:**
1. JWT stateless hai to logout kaise?
2. Refresh token chori ho jaaye to rotation kaise bachata hai?
3. Mobile app mein OAuth ke liye PKCE kyun?
4. 401 vs 403; IDOR mein 404 kyun behtar?

---

## Hafta 5: Validation, errors, logging, config

- Har boundary par validation (body, query, params, headers) zod/Joi se; **frontend validation par kabhi bharosa nahi** (VERDE: 740 MB vs 750 MB vs 500 MB limits ka mismatch isi ka example hai).
- Error classes: `AppError(code, httpStatus, isOperational)`; operational vs programmer errors.
- Logging: structured JSON (winston/pino), levels, `requestId`, PII aur tokens kabhi log mein nahi.
- Config: env vars, startup par validate (zod), `.env` git mein nahi, secrets manager ka concept.

**VERDE mein dekho:** `packages/logger`, `packages/errors`, `packages/config`: shared packages monorepo mein kaise share hote hain. Frontend `hooks/useMutation.ts` errors ko kya karta hai (review mein: sirf console).

**Exercise:** VERDE ke liye **ek jaisa error format** ka proposal likho (1 page): format, codes ki list, frontend kaise dikhayega, Cloudflare 52x par kya message.

---

## Hafta 6: Background jobs aur queues (VERDE ka core)

**Concepts:**
- Kyun queue: slow kaam (AI, PDF), retries, rate limit, spikes absorb karna.
- **Bull / BullMQ** (Redis par): job lifecycle `waiting → active → completed/failed`, `delayed`, attempts + backoff, concurrency, priority, `jobId` se dedupe, stalled jobs, repeatable jobs.
- **Idempotency:** job do baar chal sakta hai (at-least-once). Har step aisa likho ki dobara chale to nuksaan na ho (upsert, "already done?" check).
- Pause/resume/cancel: worker ke beech mein check karna padta hai (cooperative), sirf queue pause kaafi nahi.
- Fan-out: ek job → bahut saare child jobs (per file) → aggregate result.
- Dead letter / failed jobs ka handling aur alerting.

**Khud banao:** Tasks API mein `POST /exports` → BullMQ job → CSV banaye → status endpoint. Ek baar job ko beech mein crash karke dekho retry kya karta hai.

**VERDE exercise (asli bugs):**
1. `document-service/src/jobs/segregationQueue.js` padho: queue options (attempts, backoff, priority), events.
2. `document-service/src/services/segregationAdmissionService.js`: "ek project, ek job" aur credits hold ka admission logic.
3. **Bug 1, retry sab kuch dobara chalata hai:** job fail hone par saari files phir process hoti hain → dobara AI kharcha. Design karo: per-file status table (`PENDING/DONE/FAILED`) aur retry sirf `!= DONE` files par.
4. **Bug 2, resume/restart ke baad job chalta nahi:** Bull same `jobId` dobara add karne ko ignore karta hai agar purana job abhi queue mein (completed/failed) pada hai. Fix options: naya jobId (`${id}:attempt-${n}`), ya purana job remove karke add.
5. **Bug 3, pause turant nahi rukta:** worker har file ke beech DB/Redis mein `pauseRequested` check kare.
6. Har fix ka 1-page design note + test plan likho.

**Interview sawaal:**
1. At-least-once vs exactly-once; exactly-once practically kaise "jaisa" banate ho?
2. Worker crash ho gaya beech mein; job ka kya hoga (stalled)?
3. 10,000 jobs ek saath aa gaye, OpenAI rate limit 500/min: kaise control?
4. Ek user ke 1000 jobs baaki users ko block na karein (fairness, per-tenant concurrency).

---

## Hafta 7: Files, storage, uploads

- Object storage (S3 / Azure Blob): bucket/container, key naming (`org/{orgId}/project/{projectId}/...`), **pre-signed / SAS URLs** (mobile seedha storage par upload kare, server par load nahi).
- Multipart upload, resumable upload, size/type check **server par**, content hash (SHA-256) se duplicates, virus scan ka step.
- Metadata DB mein, file storage mein.
- Download: signed URL with expiry, `Content-Disposition`.

**VERDE mein dekho:** document-service ka upload flow (multer → Azure Blob), duplicate detection (naam + hash), versions/archive.

**Exercise:** Mobile-friendly upload design likho: `POST /uploads` → pre-signed URL → client seedha upload → `POST /uploads/{id}/complete` → server size/hash verify → processing job.

---

## Hafta 8: Microservices aur communication

| Topic | Kya |
|---|---|
| Kab microservices | Alag scale, alag team, alag tech (VERDE: Go for rendering, Python for ML). Kab **nahi**: chhoti team, shuruaati product |
| API gateway | Routing, auth, rate limit, CORS ek jagah (VERDE: `api-gateway` + http-proxy-middleware) |
| Sync calls | HTTP + timeout + retry + **circuit breaker** (VERDE chat-service: `opossum`) |
| Async | Events (Redis pub/sub, Kafka, SQS); event vs command; outbox pattern |
| Data | Har service ka apna data (VERDE ek shared Postgres use karta hai: trade-off samjho) |
| Tracing | OpenTelemetry: ek request kai services mein (`packages/tracing`) |

**Exercise:** VERDE ka ek flow draw karo: user "Analyze credit" dabata hai → frontend → api-gateway → ocr-service → (document-service se files) → Go document-processor → OpenAI → Postgres → notification-service → socket → frontend. Har arrow par: sync ya async, failure par kya hota hai.

**Interview sawaal:** "Service B down hai to Service A kya kare?" (timeout, retry with backoff, circuit breaker, fallback, queue).

---

## Hafta 9: Caching, rate limiting, realtime

- **Cache patterns:** cache-aside, write-through, TTL, invalidation ("do mushkil cheezein"), cache stampede (lock / early refresh), kya cache nahi karna (per-user sensitive data bina key ke).
- **Rate limiting:** fixed window, sliding window, token bucket; Redis `INCR` + `EXPIRE`; per-user, per-IP, per-tenant.
- **Realtime:** WebSocket (socket.io) rooms per project/user, horizontal scale ke liye Redis adapter, auth on connect, reconnect.

**VERDE mein dekho:** run history ka 120 s cache (page 39 bug ka hissa: cache ki wajah se naya run nahi dikhta), notification-service ka socket.io.

**Exercise:** Run history ke liye sahi invalidation design: naya run complete hone par us project ki cache key delete.

---

## Hafta 10: Testing, Docker, deploy

- **Tests:** unit (services pure rakho), integration (supertest + real Postgres via Docker/Testcontainers), contract tests ka idea. Test data factories.
- **Docker:** multi-stage Dockerfile for Node (deps → build → slim runtime), `.dockerignore`, non-root user, healthcheck, `docker compose` (api + postgres + redis).
- **CI:** GitHub Actions: install → lint → typecheck → test (services ke saath) → build image.
- **Deploy:** ek simple platform (Render/Railway/Fly) ya AWS/Azure; env vars; migrations deploy par.
- **Observability:** logs + basic metrics (request count, latency p95, error rate) + alerts.

**VERDE mein dekho:** `services/*/Dockerfile`, `Jenkinsfile` (build → ACR → Helm deploy), `packages/tracing`.

**Exercise:** Tasks API ko docker compose + GitHub Actions + deploy. README mein architecture diagram.

---

## Backend checkpoint (Month 4 end)

Bina tutorial ke ye kar paana:
- [ ] TS + Express + zod + Prisma + Postgres API, ek jaisa error format
- [ ] JWT access/refresh with rotation, RBAC, tenant scoping, IDOR test
- [ ] BullMQ job with retries, idempotent steps, status API
- [ ] Pre-signed URL upload flow
- [ ] Redis cache with invalidation, rate limiter
- [ ] Docker compose + CI + deploy
- [ ] VERDE ke 3 bugs (IDOR, retry, run-history cache) ke design notes

## Backend ke 25 interview sawaal (bol kar practice)

1. Node event loop phases; `nextTick` ka khatra.
2. Streams aur backpressure.
3. Express error handling async routes mein.
4. PUT vs PATCH; idempotent methods.
5. Cursor vs offset pagination.
6. Idempotency key kaise implement?
7. JWT vs session; JWT revoke.
8. Refresh token rotation + reuse detection.
9. OAuth2 + PKCE flow.
10. IDOR kya hai, kaise rokoge systematically?
11. Rate limiter design (token bucket) Redis ke saath.
12. Cache-aside; invalidation; stampede.
13. Queue kyun; at-least-once; idempotent consumer.
14. Worker crash beech mein.
15. Circuit breaker ki states (closed/open/half-open).
16. Microservices vs monolith: aapki choice ek naye startup ke liye.
17. Outbox pattern kya problem solve karta hai?
18. File upload via pre-signed URL ke fayde.
19. Websocket ko 10 servers par scale kaise?
20. Config aur secrets management.
21. Structured logging + request ID kyun?
22. p95 latency badh gayi: debug steps.
23. N+1 problem API layer par.
24. Graceful shutdown (in-flight requests, queue workers).
25. Backward-compatible API change kaise karoge jab purane mobile apps abhi bhi chal rahe hain?
