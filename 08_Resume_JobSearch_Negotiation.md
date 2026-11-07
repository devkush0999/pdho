# 08. Resume, LinkedIn, Job Search, Interviews aur Negotiation

> Skills se interview milta hai; **resume, referrals aur negotiation se salary**. Bahut se achhe engineers yahan 5–8 LPA chhod dete hain.

---

## 1. Resume (1 page, ATS-friendly)

### 1.1 Template

```
NAAM
Mobile Engineer (Android/iOS/Flutter/RN) · Backend (Node.js, PostgreSQL) · LLM integration
City · phone · email · linkedin.com/in/… · github.com/… · portfolio/demo link

SUMMARY (2–3 lines)
Mobile engineer with 3+ years building and shipping <stack> apps used by <N> users.
Build end-to-end features across mobile, Node.js/PostgreSQL backends and LLM-powered
document workflows; focused on offline-first, performance and reliability.

SKILLS
Mobile:   Kotlin, Jetpack Compose, Coroutines/Flow, Room, WorkManager, Hilt   (apna stack)
Backend:  Node.js, TypeScript, Express/Fastify, PostgreSQL, Prisma, Redis, BullMQ, REST
AI:       LLM APIs (structured outputs), RAG with pgvector, evaluation, on-device ML (ML Kit)
Tools:    Docker, GitHub Actions, Firebase, Sentry, Git

EXPERIENCE
Company — Mobile Engineer                                   MMM YYYY – Present
• <Action verb> <kya> using <tech>, resulting in <number>.
• … (4–6 bullets, sabse impactful upar)

PROJECTS
DocSnap — Mobile + Node.js + PostgreSQL + LLM                  github · demo
• Offline-first scanning with outbox + background upload; resumes after app kill.
• Idempotent BullMQ workers; LLM extraction with JSON schema + code checks; 78% → 91% accuracy on 40-case eval.
• RAG chat over user documents with pgvector and page citations; streaming UI.

EDUCATION
Degree, College, Year
```

### 1.2 Bullet formula: **Action + What + How (tech) + Result (number)**

| ❌ Kamzor | ✅ Strong |
|---|---|
| Worked on the login module | Rebuilt auth with token rotation and a single-flight refresh for concurrent 401s, cutting forced logouts by ~90% |
| Responsible for app performance | Reduced cold start from 2.8s to 1.6s with lazy SDK init and Baseline Profiles; Play vitals "slow start" warning cleared |
| Fixed bugs | Raised crash-free users from 98.9% to 99.7% by fixing top 12 Crashlytics issues (OOM in image feed, lifecycle leaks) |
| Did offline support | Built offline-first sync (Room + WorkManager outbox) so 40k field users could submit forms without network; failed submissions ~0 |
| Worked with backend team | Designed paginated REST contract with backend team, cutting feed payload 60% and API calls per session 35% |
| Used AI | Shipped receipt-scan feature: on-device OCR + LLM extraction; manual entry time down from ~2 min to ~15 s |

**Number kahan se laayein:** Play Console / App Store Connect vitals, Crashlytics, analytics, PR descriptions, Jira, apna estimate (honest, "~" ke saath). **Jhooth mat likhiye**; har number par interviewer "kaise napa?" poochega.

### 1.3 Action verbs
Built, Designed, Shipped, Reduced, Improved, Migrated, Automated, Led, Owned, Optimized, Introduced, Replaced, Scaled, Debugged, Mentored.

### 1.4 Resume ki galtiyan
- 2+ pages, photos, objective statement, "hardworking team player".
- Skills mein 40 cheezein (jo poocha jaaye aur na aaye wo minus hai).
- Sirf zimmedariyan, result nahi.
- PDF ke alawa format; fancy 2-column templates (ATS parse nahi kar pata).
- File naam: `Firstname_Lastname_Mobile_Engineer.pdf`.

---

## 2. LinkedIn

| Section | Kya likhna |
|---|---|
| Headline | `Mobile Engineer · Kotlin/Compose · Node.js · LLM-powered apps · Open to SDE-2 roles` |
| About (5–6 lines) | Kya banate ho, kis cheez mein achhe ho, 2 achievements number ke saath, kya dhoondh rahe ho |
| Featured | DocSnap demo video, GitHub, ek blog post |
| Experience | Resume wale bullets |
| Open to work | Recruiters only (current employer se chhupa) |
| Activity | Har 2 hafte ek post: project progress, ek bug ki kahani, ek seekh. Recruiters active profiles zyada dekhte hain |

---

## 3. Job search ki strategy

### 3.1 Target list (30–40 companies, 3 tiers)

| Tier | Kaun | Kitni | Kyun |
|---|---|---|---|
| Practice | Chhote startups, jahan offer milna aasaan | 8–10 | Interview ka darr nikalna, feedback |
| Target | Funded product startups / mid-size SaaS, fintech, AI startups (18–30 LPA band) | 15–20 | **Asli goal** |
| Stretch | Top product cos | 5–8 | Mil gaya to bonus; leverage |

Har company ke liye spreadsheet: `Company | Role | Link | Referral person | Applied date | Stage | Next step | Notes`.

### 3.2 Kahan dhoondhein
LinkedIn Jobs (filters: past week, experience 2–5 yrs), Instahyre, Wellfound (startups), Cutshort, Naukri (profile updated rakhein, recruiters wahan search karte hain), company careers pages, Hacker News "Who is hiring", tech communities/Discord/Twitter(X).

### 3.3 Referral messages (sabse zyada asar)

**Engineer ko (LinkedIn, chhota):**
```
Hi <Name>, I'm a mobile engineer (3 yrs, <stack>) and I've been following <Company>'s
<product/feature>. I'm applying for the <Role> (link). I recently built <DocSnap: one line>
(github link). Would you be open to referring me, or sharing what the team looks for?
Happy to send my resume. Thanks!
```

**Recruiter ko:**
```
Hi <Name>, I saw the <Role> opening. Quick summary: 3 yrs shipping <stack> apps
(<1 strong number>), plus Node.js/Postgres and LLM features (<project link>).
I'd love to be considered. Resume attached. Available for a call this week.
```

**Rules:** personalize 1 line; ek hafte baad 1 follow-up; 10 messages/hafte ka target; jawab na aaye to bura mat maano.

### 3.4 Timing
- Saare interviews **2–4 hafte ki window** mein rakhne ki koshish (offers saath aayein → negotiation power).
- Practice companies pehle, target companies 2–3 hafte baad.
- Notice period (60–90 din) bata do; "negotiable/buyout possible" agar ho.

---

## 4. Interview process (typical product company)

| Round | Kya | Taiyari |
|---|---|---|
| Recruiter screen | Experience, notice, expectations | 60-second intro (neeche) |
| Online assessment | 2–3 DSA (60–90 min) | File 05 |
| DSA / coding | 1–2 medium | File 05 |
| Mobile / domain deep dive | Stack internals, project | File 01 |
| LLD / mobile system design | 45–60 min | File 06 |
| HLD (kabhi kabhi) | 45–60 min | File 06 |
| Hiring manager / behavioral | Ownership, conflicts, impact | Section 5 |

### 4.1 "Tell me about yourself" (60–90 second)
```
"I'm a mobile engineer with 3 years of experience, currently at <Company> building
<product> for <users>. Most of my work is <stack>; I owned <feature> where I <achievement + number>.
Over the last year I've gone deeper into backend and AI: I built DocSnap, an offline-first
scanning app with a Node.js/Postgres backend and LLM extraction, where I <1 interesting thing>.
I'm looking for a role where I can own features end-to-end across mobile and backend,
which is why <Company>'s <specific thing> interests me."
```

---

## 5. Behavioral: STAR stories (8 stories taiyar rakhein)

**STAR:** Situation (1–2 line) → Task (aapki zimmedari) → Action (**aapne** kya kiya, "we" nahi) → Result (number) → Learning.

| # | Story type | Kin sawaalon mein kaam aayegi |
|---|---|---|
| 1 | Sabse mushkil technical problem / bug | "Hardest bug", "technical challenge" |
| 2 | Performance improvement | "Impact", "optimization" |
| 3 | Production incident jo aapne handle kiya | "Pressure", "ownership" |
| 4 | Disagreement (PM/designer/senior) | "Conflict", "pushback" |
| 5 | Galti jo aapne ki | "Failure", "what would you do differently" |
| 6 | Kuch naya seekh kar deliver kiya (backend/AI) | "Learning", "ambiguity" |
| 7 | Kisi ki madad / mentoring | "Leadership", "teamwork" |
| 8 | Scope/deadline trade-off | "Prioritization" |

**Example (story 3):**
> **S:** Release ke 2 ghante baad crash-free rate 99.6% se 97% gira. **T:** Main release owner tha. **A:** Staged rollout 10% par roka, Crashlytics se dekha crash ek naye image-decoding path mein low-RAM devices par hai; feature flag se feature band kiya (bina naya release), phir downsampling fix + test low-end emulator par, aur 2 din mein hotfix. **R:** 30 min mein impact rok diya, hotfix ke baad 99.7%. **L:** Ab har release ke liye low-end device test aur feature flags default hain.

---

## 6. Salary negotiation

### 6.1 Pehle apna number tay karo
- Market research: Levels.fyi (India), AmbitionBox, Glassdoor, dosto se. Role + city + company stage.
- 3 numbers: **walk-away** (isse kam nahi, e.g. 16), **target** (20), **ask/anchor** (23–24).
- **Fixed** par focus; variable, joining bonus, ESOPs alag ginno.

### 6.2 "Current CTC kya hai?" / "Expectation kya hai?"
India mein current CTC aksar poocha jaata hai. Jhooth mat bolo (offer letter/payslip maang sakte hain), lekin focus expectation par le jaao:
```
"My current CTC is 8 LPA, but I've grown a lot in scope — I'm now working across mobile,
backend and AI features. Based on the role and market data for SDE-2 mobile/full-stack,
I'm looking at 22–24 LPA fixed. I'm flexible on structure for the right role."
```
Agar bolo ki 8 se itna jump kaise: "My current pay reflects my first role; I'm being evaluated for SDE-2 level work here, and I'd expect the offer to reflect this role's band rather than my previous salary."

### 6.3 Offer aane par
1. **Turant haan mat bolo.** "Thank you, I'm excited. Can I have 2–3 days to review the details?"
2. Breakdown maango: fixed, variable (kitna guaranteed?), joining bonus, ESOPs (vesting, cliff, strike price), notice buyout, relocation.
3. Counter (email/call):
```
"I'm really excited about the role and the team. I have another process at the final stage
in the 22–25 LPA range. If you can bring the fixed component to 22 LPA, I'm ready to
sign this week."
```
(Doosra offer sach mein ho tabhi bolo. Na ho to: "Based on market data for this role and the scope we discussed, I was expecting around 22 fixed.")
4. Fixed nahi badhe to: joining bonus, notice buyout, early review (6 mahine), ESOP refresh maango.
5. Sab likhit (email/offer letter) mein lo.

### 6.4 Offers compare karna
| Factor | Offer A | Offer B |
|---|---|---|
| Fixed | | |
| Variable (realistic) | | |
| Joining bonus | | |
| ESOP (realistic value, vesting) | | |
| Role / tech / learning | | |
| Team + manager impression | | |
| Company stability (funding, revenue) | | |
| WLB / location / remote | | |

20 LPA ke liye **learning wali company** kabhi kabhi 1–2 LPA kam par bhi behtar hai, kyunki agla jump (2 saal baad) usi se hota hai.

---

## 7. Interview ke baad
- Har round ke baad 10 minute: kya poocha, kya galat hua, notes. Ye agla interview behtar karta hai.
- Reject hue to recruiter se politely feedback maango; 6 mahine baad dobara apply kar sakte ho.
- 1 reject = 1 data point. Pehle 5–10 interviews mein reject normal hai.

---

## 8. Month 8–9 ka hafta-wise plan

| Hafta | Kaam |
|---|---|
| 1 | Resume final (2 logon se review), LinkedIn update, target sheet (35 companies), 60-sec intro + 8 STAR stories likho |
| 2 | 10 referral messages + 10 applications (practice tier); 2 mocks |
| 3 | Practice interviews; har round ke notes; 10 aur messages (target tier) |
| 4 | Target tier applications; DSA revisit list; 2 mocks |
| 5–6 | Target interviews; follow-ups; system design revision |
| 7 | Offers → negotiation; stretch companies ko offer ka update ("I have an offer, timeline X") |
| 8 | Decide, sign, resignation, notice period plan, knowledge transfer achhe se (bridge mat jalao) |
