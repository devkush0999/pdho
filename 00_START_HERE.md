# 8 LPA → 20 LPA: Deep Roadmap (yahan se shuru karein)

> Aapka profile: Mobile app developer · 3 saal · 8 LPA · target 20 LPA (SDE-2 / Senior Mobile / Full-stack Mobile + AI).
> Ye folder ek **poora syllabus** hai: har file ek area ko week-by-week, exact topics, exact sawaal aur exact practice ke saath cover karti hai.
> Overview chahiye to: [../Career_Roadmap_Mobile_to_20LPA.md](../Career_Roadmap_Mobile_to_20LPA.md)

---

## 1. Files ka kram

| # | File | Kya milega | Kab |
|---|---|---|---|
| 0 | **Ye file** | Self-assessment test, roz ka system, tracker | Day 1 |
| 1 | [01_Mobile_Deep.md](01_Mobile_Deep.md) | Android / iOS / Flutter / React Native: SDE-2 level ka har topic + 60 interview sawaal + mobile system design | Month 1–2 (aur hamesha) |
| 2 | [02_Backend_Deep.md](02_Backend_Deep.md) | Node.js + TypeScript backend: 10 hafte, VERDE ki asli files par exercises | Month 2–4 |
| 3 | [03_Database_Deep.md](03_Database_Deep.md) | PostgreSQL, Prisma, Redis, pgvector: SQL exercises, indexes, transactions, interview sawaal | Month 2–4 |
| 4 | [04_AI_Engineering_Deep.md](04_AI_Engineering_Deep.md) | LLM APIs, structured output, RAG, evaluation, mobile + AI; code ke saath | Month 3–5 |
| 5 | [05_DSA_Deep.md](05_DSA_Deep.md) | 16 patterns, ~170 named LeetCode problems, templates, practice method | Roz, Month 1–9 |
| 6 | [06_System_Design_Deep.md](06_System_Design_Deep.md) | LLD, HLD, mobile system design: framework + 3 poore worked examples | Month 3–8 |
| 7 | [07_Flagship_Project_Spec.md](07_Flagship_Project_Spec.md) | Project ka poora spec: features, DB schema, APIs, AI, screens, 10-hafte milestones | Month 4–7 |
| 8 | [08_Resume_JobSearch_Negotiation.md](08_Resume_JobSearch_Negotiation.md) | Resume template, LinkedIn, referral messages, behavioral answers, negotiation scripts | Month 6–9 |

---

## 2. Day 1: apna honest self-assessment (45 minute)

Har sawaal ka jawab **bina Google kiye** zor se bol kar dijiye. 0 = nahi aata, 1 = thoda, 2 = confidently samjha sakta hoon.

### A. Mobile depth (aapke stack ke hisaab se)
| # | Sawaal | 0/1/2 |
|---|---|---|
| 1 | Aapke framework mein UI dobara kab aur kyun banta hai (recomposition / re-render / rebuild)? Isko kam kaise karte ho? | |
| 2 | Main thread kyun block nahi karna chahiye? Aapke stack mein background kaam ka sahi tareeka kya hai? | |
| 3 | App process mar jaaye (process death / app killed) to user ka state kaise bachaoge? | |
| 4 | Do API calls ek saath 401 dein to token refresh sirf ek baar kaise karoge? | |
| 5 | Offline mein user ne data badla, online aane par server par kisi aur ne bhi badla. Kya karoge? | |
| 6 | App start time kaise napoge aur kaise kam karoge? | |
| 7 | Memory leak kaise dhoondhoge? Ek real example do | |
| 8 | Push notification ka poora flow: server → FCM/APNs → device → app (foreground/background/killed) | |

### B. Backend + DB
| # | Sawaal | 0/1/2 |
|---|---|---|
| 9 | REST mein PUT vs PATCH, 401 vs 403, 409 vs 422 | |
| 10 | JWT access + refresh token flow; refresh token chori ho jaaye to? | |
| 11 | Index kya hai, composite index `(a, b)` par `WHERE b = ?` chalega? | |
| 12 | Transaction aur isolation; do log ek saath wallet se paise kaatein to? | |
| 13 | Redis cache-aside pattern; cache invalidate kab karoge? | |
| 14 | Queue kyun chahiye? Job do baar chal jaaye to kya hoga (idempotency)? | |

### C. AI
| # | Sawaal | 0/1/2 |
|---|---|---|
| 15 | LLM se guaranteed JSON kaise lete ho, aur galat aaye to? | |
| 16 | RAG kya hai, embeddings kya hain, chunk size kaise chunoge? | |
| 17 | Prompt badla to kaise pata chalega ki result behtar hua ya kharab? | |

### D. Interview core
| # | Sawaal | 0/1/2 |
|---|---|---|
| 18 | 30 minute mein LeetCode medium (jaise "Longest Substring Without Repeating Characters") solve kar lete ho? | |
| 19 | "Design Instagram feed (mobile app)" par 35 minute structured bol sakte ho? | |
| 20 | "Apne career ka sabse mushkil bug" STAR format mein 2 minute mein? | |

**Score (max 40):**

| Score | Matlab | Timeline |
|---|---|---|
| 30–40 | Aap kareeb ho; interviews 2–3 mahine mein | 3–4 mahine |
| 18–29 | Normal starting point | 6–7 mahine |
| 0–17 | Neev se kaam | 8–10 mahine |

Har 2 mahine baad yahi test dobara. Score ka graph hi aapka progress hai.

---

## 3. Roz ka system (job ke saath)

**Golden rule:** padhna 30%, banana/solve karna 70%.

| Samay | Kaam | Rules |
|---|---|---|
| Subah 45 min (office se pehle) | **DSA** | 1 problem. 25 min khud, phir solution, phir bina dekhe dobara likho. Notebook mein pattern likho |
| Raat 75 min | **Us mahine ka skill** (file 1–4) | 20 min padhna → 55 min code. Har din kuch commit ho |
| Lunch break 15 min | Revision | Pichhle din ke notes / flashcards (Anki) |
| Saturday 5 ghante | Project / VERDE exercise | Ek poora feature end-to-end |
| Sunday 3 ghante | System design (1.5 h) + weekly review (30 min) + rest | Review: kya hua, kya nahi, agle hafte ke 3 goals |

**Hafte mein ek din off.** Burnout se 9 mahine ka plan 3 mahine mein toot jaata hai.

### Notes ka tareeka
- `learning-log` naam ka GitHub repo banao. Har din ek line: date, kya seekha, link.
- Har topic ke baad **"5 line explanation"** apne words mein. Agar 5 line mein nahi samjha pa rahe to samajh nahi aaya.
- Interview sawaal Anki flashcards mein (sawaal → 3 bullet jawab).

---

## 4. Tracker (copy karke use karein)

### Monthly
| Mahina | Focus | Target | Done? |
|---|---|---|---|
| 1 | Mobile depth + DSA arrays/strings/hashing | Self-test 1; 30 DSA; 1 improvement current app mein | |
| 2 | Mobile testing/perf + Backend shuru + DSA two pointers/window/stack | 60 DSA; Node+TS CRUD API | |
| 3 | Backend + DB + DSA trees/BS | 90 DSA; VERDE exercise 1–4 | |
| 4 | Backend + AI shuru + LLD | 115 DSA; VERDE exercise 5–8; LLD 3 problems | |
| 5 | AI + project shuru + DSA graphs | 135 DSA; RAG demo; project milestone 1–2 | |
| 6 | Project + mobile system design + DSA DP | 155 DSA; project milestone 3–6; self-test 2 | |
| 7 | Project finish + HLD | 170 DSA; project live; 3 HLD practice | |
| 8 | Resume + mocks + applications (practice companies) | 5 mocks; 15 applications; 10 referral messages | |
| 9 | Target companies, offers, negotiation | 2+ offers | |

### Weekly review template
```
Hafta: __
DSA solved: __ (pattern: ___)
Skill hours: __
Kya banaya/commit kiya: ___
Kahan atka: ___
Agle hafte ke 3 goals: 1) ___ 2) ___ 3) ___
Energy (1-5): __
```

---

## 5. Agar time kam pad jaaye to kya chhodna (priority)

| Kabhi mat chhodo | Kam kar sakte ho | Baad mein |
|---|---|---|
| DSA roz | HLD ki gehrai (basic kaafi) | Go, Kubernetes ki gehrai |
| Mobile depth | AI ka RAG part (structured output zaroori) | Python / ML |
| Backend + Postgres | Project ke extra features | Microservices ki gehrai |
| Ek flagship project | | |
| Resume + referrals | | |
