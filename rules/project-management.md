\# Marcus Library — rules/project-management.md

\# Commander's Workflow — From Raw Opportunity to Shipped Revenue

\# Schema: 1.0.0

\# Auto-loaded on: "new project", "should I build", "opportunity", "what should I work on",

\#                  "help me plan", "we need to build X", "is this worth it"



\---



\## PHILOSOPHY



Projects die at two moments: before they start (analysis paralysis) and after they start

(execution drift). The Commander's Workflow exists to prevent both.



It is not a process for its own sake. It is a forcing function that answers three questions:

\*\*Is this worth building?\*\* (Phase 0)

\*\*What exactly are we building?\*\* (Phase 1)

\*\*How do we build it without losing the thread?\*\* (Phases 2–5)



Every phase produces a written artifact. No artifact = phase incomplete = cannot proceed.

The artifact is not bureaucracy. It is the only way to make decisions made under clarity

available to future-Karl who will be operating under stress, time pressure, and context loss.



\---



\## THE FIVE PHASES

Phase 0 → opportunity\_assessment.md   (Is this worth it?)

Phase 1 → technical\_blueprint.md      (What exactly are we building?)

Phase 2 → tasks.md                    (How do we break it into executable pieces?)

Phase 3 → \[execute + commit log]      (Build it, one task at a time)

Phase 4 → \[review + pentest]          (Is it production-ready?)

Phase 5 → \[ship + monitor + iterate]  (Is it actually solving the problem?)



\*\*Gate rule:\*\* Each phase requires the previous phase's artifact to exist and pass its

completion test. No skipping. No partial progression. No "we'll formalize it later."



The reason: every shortcut taken at Phase 0 costs 10x to fix at Phase 3. Every missing API

contract in Phase 1 costs 10x in integration bugs at Phase 3. This math is not theoretical —

it is what happened in every project Karl has seen get stuck.



\---



\## PHASE 0 — OPPORTUNITY ASSESSMENT



\*\*Artifact:\*\* `opportunity\_assessment.md`

\*\*Time budget:\*\* 1–4 hours for a feature. 1–3 days for a product. Never more.

\*\*Failure mode:\*\* Over-planning paralysis. If this document exceeds 3 pages, you are writing

to avoid building. Cut it.



\---



\### 0.1 — Problem Statement



Write one sentence from the target user's perspective.



Format: "\[User type] struggles to \[do X] because \[root cause], which costs them \[consequence]."



Real example from minadoor-db:

"Travel agency staff struggle to manage client passport data and track travel applications

because everything is in WhatsApp and spreadsheets, which causes missed renewals and

duplicate client entries."



\*\*Completion test:\*\* Can Marcus describe this problem to a stranger in one sentence and have

them immediately understand why someone would pay to solve it? Yes → proceed. No → rewrite.



Anti-patterns:

\- "Users want a better experience" — not a problem statement, it is a wish

\- "The market is large" — market size is not a problem

\- "I thought of this" — motivation is not validation



\---



\### 0.2 — Target Customer (Named and Described)



Not "travel agencies." Not "businesses." A specific person.



Required fields:

Name (persona): \[name — use a real-sounding name, it forces specificity]

Role: \[their actual job title]

Company size: \[1-person operation / 5-person agency / enterprise]

Their current workflow: \[exactly what they do today without your product]

Their biggest daily frustration: \[the thing that makes them say "merde" under their breath]

Tech comfort: \[Excel user / comfortable with web apps / developer]

Where they are: \[Oran / Algiers / specific geography — matters for payment and UX]

How they'd find you: \[word of mouth / Instagram / Google / cold outreach]



\*\*Completion test:\*\* Marcus could write a 3-sentence cold message to this person that doesn't

sound generic. If it reads like a MailChimp template, the persona is not specific enough.



\---



\### 0.3 — Solution Hypothesis



One sentence. No adjectives. No marketing language.



Format: "We build \[specific thing] that lets \[user] \[do specific action] \[in what context]."



Real example: "We build a web CRM that lets travel agencies import client passport data from

Excel, track application status, and export formatted reports — replacing their WhatsApp

spreadsheet workflow."



\*\*What this is NOT:\*\* "We build an AI-powered intelligent platform that revolutionizes..."

That sentence contains zero information. Strip every adjective that isn't a technical spec.



\---



\### 0.4 — Success Criteria (Measurable, Time-Bound)



Three metrics. Each must be: measurable with a number, achievable in a defined timeframe,

actually indicative of the product working (not vanity metrics).



Format:

M1: \[N] \[metric] by \[date] — \[why this metric proves the product is working]

M2: \[N] \[metric] by \[date]

M3: \[N] \[metric] by \[date]



Real example:

M1: 3 paying agencies using the product within 60 days — proves real users will pay

M2: €300 MRR within 90 days — proves the pricing model works

M3: <2% monthly churn within 120 days — proves it solves the problem well enough to keep



Rejected metrics: "good feedback", "people like it", "traction", "growing user base",

"high engagement." These are feelings, not measurements.



\---



\### 0.5 — Risk Inventory (Top 3 + Mitigations)



Not "maybe the market is wrong." Specific risks with specific mitigations.



Format:

R1: \[specific risk] — probability: \[H/M/L] — impact: \[H/M/L]

Mitigation: \[specific action that reduces this risk]

Tripwire: \[observable signal that this risk is materializing — triggers re-evaluation]

R2: ...

R3: ...



Real example from minadoor-db context:

R1: Algerian agencies won't pay for software when WhatsApp is free — P:H, I:H

Mitigation: charge per-import (transactional model) to reduce perceived risk vs. monthly sub

Tripwire: <3 conversions from 20 demos → pivot pricing model

R2: Excel import format varies too much per agency to handle reliably — P:M, I:H

Mitigation: i18n header alias map + preview step that shows parsed data before commit

Tripwire: >20% of imports fail in first month → rebuild parser with fuzzy matching

R3: Competition from established CRM players (Zoho, HubSpot) — P:L, I:M

Mitigation: hyper-local (Arabic/French/Darja UI, Algerian travel types, DZD pricing)

Tripwire: if competitor adds Arabic interface within 6 months → accelerate differentiators



\---



\### 0.6 — Build vs. Buy vs. Ignore



For every major component, answer:



| Component | Build | Buy/Use OSS | Ignore for v1 |

|-----------|-------|-------------|---------------|

| \[component] | \[reason to build] | \[specific tool + cost] | \[why skip] |



Real rule: \*\*default to buy/OSS unless Karl needs it as a core competency or competitive

differentiator.\*\* Building authentication from scratch is not a differentiator. Building

the fastest import engine for Algerian travel data formats is.



\---



\### 0.7 — Realistic Timeline

Raw estimate: \[N days/weeks]

Adjusted estimate: \[N × 2.5x] = \[final estimate]

Phase breakdown:

Phase 0–1 (spec + blueprint): \[N days]

Phase 2–3 (implementation): \[N days]

Phase 4 (review + pentest): \[N days]

Phase 5 (ship + first iteration): \[N days]

Buffer (20%): \[N days]

Total: \[sum]



No raw estimates in any public communication. The adjusted estimate is the estimate.



\---



\### 0.8 — Go / No-Go Gate



Marcus evaluates the completed assessment against three questions:

G1: Is the problem real? (Has at least one real human confirmed this frustration exists?)

\[Yes / No / Inferred — needs validation]

G2: Is the solution minimal enough to ship in the estimated time?

\[Yes / No — if No: cut scope until Yes]

G3: Does this align with Karl's current priority stack?

\[Yes / No / Conflict with: \[current active project]]



All three Yes → proceed to Phase 1.

Any No → address the No before proceeding. Do not override the gate.

Conflict detected → invoke BIAS-03 protocol (Shiny Object). Park the idea.



\---



\## PHASE 1 — TECHNICAL BLUEPRINT



\*\*Artifact:\*\* `technical\_blueprint.md`

\*\*Time budget:\*\* 4–8 hours for a feature module. 2–5 days for a full product.

\*\*Failure mode:\*\* Blueprint becomes a novel. If API contracts aren't written, the blueprint

is not complete regardless of how detailed the architecture diagrams are.



\---



\### 1.1 — Architecture Diagram



ASCII is sufficient. Requirements:

\- Every service box is labeled with its technology

\- Every arrow is labeled with its protocol (HTTP, WebSocket, Redis pub/sub, etc.)

\- Every external dependency is visible (third-party APIs, payment processors, email)

\- Every async boundary is marked (what is synchronous vs. queued vs. background)



Real example from minadoor-db:

\[Browser: React/Vanilla JS]

↕ HTTPS/JSON

\[Nginx reverse proxy]

↕ HTTP

\[FastAPI Application]

↕ asyncpg          ↕ aioredis

\[PostgreSQL 15]       \[Redis 7]

(RLS enabled)      (job queue + import cache)



If you cannot draw the architecture in ASCII, you do not understand it yet.

Understanding it is the prerequisite for implementing it.



\---



\### 1.2 — Data Model (Complete, Before Code)



For every entity:

Table: \[name]

Purpose: \[one sentence]

Fields:



\[name]: \[type] \[nullable/required] \[default] \[constraints] — \[why this field exists]

Indexes:

\[field(s)]: \[reason — what query pattern this serves]

Relationships:

\[FK to table.field] \[on\_delete behavior]

Multi-tenancy: \[which field is the tenant partition key? How is RLS enforced?]





\*\*The question that must be answered for every table:\*\*

"If two requests from different tenants arrive simultaneously, is there any way one

can read or modify the other's data?" If the answer is "I'm not sure" → stop and design

the isolation before writing any code.



\---



\### 1.3 — API Contracts (The Most Important Section)



For every endpoint that will be implemented:

METHOD /api/v{version}/path

Purpose: \[one sentence]

Auth: \[none | JWT Bearer | API key | session] — \[what scope/permission required]

Rate limit: \[N requests/minute per user] \[N requests/minute per IP]

Request:

Headers: \[any required headers]

Path params: { \[name]: \[type] \[description] }

Query params: { \[name]: \[type] \[optional/required] \[description] }

Body: {

\[field]: \[type] (\[required|optional]) — \[validation rules] — \[description]

}

Response 200/201:

{

\[field]: \[type] — \[description]

}

Response 400 (validation error):

{ error: "VALIDATION\_ERROR", code: "INVALID\_\[FIELD]", details: { \[field]: \[message] } }

Response 401: { error: "UNAUTHORIZED", code: "INVALID\_TOKEN" }

Response 403: { error: "FORBIDDEN", code: "INSUFFICIENT\_PERMISSIONS" }

Response 404: { error: "NOT\_FOUND", code: "\[RESOURCE]\_NOT\_FOUND" }

Response 422: { error: "UNPROCESSABLE", code: "\[reason]", details: any }

Response 500: { error: "INTERNAL\_ERROR", code: "SERVER\_ERROR" } — no stack traces to client

Side effects:

DB writes: \[what gets inserted/updated/deleted]

Cache: \[what gets invalidated or written]

Events: \[what gets emitted to queue]

External calls: \[what third-party APIs are called]

Security:

STRIDE analysis: \[S/T/R/I/D/E — one sentence each for relevant threats]

Input validation: \[what gets validated and how]

Authorization check: \[how we verify this user can act on this specific resource]



\*\*Completion test:\*\* Can Marcus write the integration test for this endpoint using

only this contract, without reading the implementation? Yes → contract is complete.

No → contract is incomplete, do not proceed.



\---



\### 1.4 — Technology Decisions Log



For every non-trivial technology choice:

Decision: \[what was chosen]

Alternatives considered: \[what else was evaluated]

Reason chosen: \[the specific technical or business reason]

Tradeoffs accepted: \[what are we giving up by making this choice?]

Migration path: \[if this turns out to be wrong, how do we get out?]

Review trigger: \[what condition would make us revisit this decision?]



Real example:

Decision: PostgreSQL 15 with Row Level Security for multi-tenancy

Alternatives: SQLite (too limited for concurrent writes), separate DB per tenant (too expensive)

Reason: RLS enforces isolation at DB level — app bugs cannot cross tenant boundaries

Tradeoffs: RLS policies add query complexity, harder to debug

Migration path: RLS can be disabled and replaced with app-level WHERE clauses if complexity grows

Review trigger: >10 tenants with complex permission needs → evaluate dedicated schemas



\---



\## PHASE 2 — TASK BREAKDOWN



\*\*Artifact:\*\* `tasks.md`

\*\*Format:\*\* Exactly as established in minadoor-db (proven, working — do not reinvent).



\---



\### 2.1 — Task Anatomy (Canonical)



&#x20;T\[N]: \[verb phrase — specific action, not a noun]

Acceptance: \[done when X is true — testable, single condition]

Files: \[list of files to create or modify]

Estimated: \[raw]h → \[raw × 2.5x]h adjusted

Depends: \[T-IDs or "none"]

Max LOC: \[target, usually 30–50]





\### 2.2 — Task Rules (Hard)



\*\*One task = one commit.\*\* No exceptions. If two things changed in one commit, they should

have been two tasks. This is not pedantry — it makes rollback precise and review meaningful.



\*\*Tasks are independent at execution time.\*\* Dependencies must be complete before a task

starts, not in progress. "T8 can start when T7 is halfway done" is not a valid plan.



\*\*Every implementation task is paired with a test task.\*\* Either in the same task (for simple

functions where test is <20 lines) or as an explicitly numbered follow-up (T8 implement,

T8a test). There is no implementation without a test. The test is part of the definition of done.



\*\*No task touches more than 3 files.\*\* More than 3 = scope too large = decompose further.

Exception: renaming/moving operations that change N files uniformly (a refactor task).



\*\*Acceptance criteria are binary.\*\* "Works correctly" is not binary. "pytest tests/test\_import\_parser.py

passes with 0 failures" is binary. Write the test before the implementation so the acceptance

criteria writes itself.



\---



\### 2.3 — Task Sequencing Rules

Order of operations for any new module:



Database migration (if schema change needed) — always first

Pydantic schemas / TypeScript types — before any business logic

Repository / data access layer — before services

Service layer — before routes

Route handlers — after service layer is tested

Frontend — after API contract is verified working

Integration tests — after all layers are in place

Security audit task — always last before Phase 4



Anti-pattern: building frontend before the API contract is verified.

This is the most common source of rework in minadoor-db-class projects.

The frontend will be built twice. Every time.



\---



\## PHASE 3 — EXECUTION



\*\*No artifact.\*\* Execution is measured by git log, not by documents.



\---



\### 3.1 — Execution Rules



\*\*One task at a time.\*\* No context switching mid-task. Task started → task completed → commit

→ only then start next task. Incomplete tasks in parallel produce half-working features,

which are worse than no features because they hide actual progress.



\*\*The 30-minute stuck rule.\*\* If blocked for >30 minutes on a single task: do not spiral.

State the block to Marcus: `\[BLOCK] T\[N]: \[exactly what I tried, what happened, what I expected]`.

This produces a targeted response instead of a debugging exploration that takes 4 hours.



\*\*No premature optimization.\*\* Make it work, then make it right, then make it fast — in that order,

only if measurement shows it needs to be fast. Optimizing code that isn't correct first is

the most expensive form of waste in software development.



\*\*Commit message discipline.\*\* As defined in rules/coding.md. Every commit message answers:

"What changed and why?" not "What files I edited."



\---



\### 3.2 — Daily Execution Cadence

Session start:



Read current tasks.md — where am I?

Pick the next incomplete task in sequence (no cherry-picking easier tasks)

Run existing tests — confirm green baseline before touching anything

Start task



Session end:



Commit whatever is complete (even partial — commit the test if implementation isn't done)

Update tasks.md checkbox

If blocked: write the block description in tasks.md under the task

Note: what is the first action next session?





\---



\### 3.3 — When to Deviate from the Plan



The blueprint is a plan, not a contract with reality. Deviate when:

\- New information makes the plan objectively wrong (discovered a constraint that breaks the design)

\- A task reveals a dependency that wasn't in the blueprint



Do not deviate when:

\- The task is harder than expected (this is the 2.5x multiplier at work — proceed)

\- A new feature idea emerges (park it in reference/ideas.md — BIAS-03 protocol)

\- The current design feels "inelegant" (elegance is refactoring-phase work, not execution-phase work)



When deviation is necessary: update the blueprint first, then continue. Do not let the

blueprint and the code diverge silently. A blueprint that doesn't match the code is worse

than no blueprint.



\---



\## PHASE 4 — REVIEW AND HARDENING



\*\*Trigger:\*\* All tasks in tasks.md checked, or end of a defined phase.



\---



\### 4.1 — Code Review Pass



Load rules/coding.md → CODE REVIEW section → run full P0/P1/P2/P3 checklist on every file changed.



This is not optional before shipping. It is the difference between software that works today

and software that works in 6 months under load with unfamiliar edge cases.



P0 failures: fix immediately. No negotiation.

P1 failures: fix or document the accepted risk with a date for resolution.

P2/P3: log as technical debt in tasks.md with T\[N] format.



\---



\### 4.2 — Security Review Pass



Load rules/security.md → PHASE II and PART V → run against the feature's attack surface.



Minimum required:

□ STRIDE completed for every new endpoint

□ OWASP checklist reviewed for this release (relevant items only — not all 10 for a CSS change)

□ No new secrets in code or git history (gitleaks scan)

□ All new user inputs validated against Pydantic schema

□ All new endpoints check authentication AND authorization (IDOR prevention)

□ Dependency audit run (pip-audit / npm audit clean on HIGH/CRITICAL)



\---



\### 4.3 — Performance Review



Measure before shipping, not after user complaints.

For every new endpoint: load test with locust or k6 (5 minutes, 50 concurrent users)

Target: p99 <500ms, error rate <0.1%, no memory growth over test duration

If target missed: profile → identify bottleneck → fix the bottleneck → re-test

Never optimize based on intuition. Always profile first.



\---



\### 4.4 — Production Readiness Gate



Load rules/coding.md → PHASE 12 → run the full checklist.

Every unchecked item is either fixed or documented with accepted risk.



\---



\## PHASE 5 — SHIP AND ITERATE



\*\*Philosophy:\*\* Shipping is not the end of the project. It is the beginning of learning.

The blueprint was a hypothesis. The real users are the experiment.



\---



\### 5.1 — First 48 Hours Protocol



Watch:

Error rate: dashboard or Telegram alert if >0.5% on any endpoint

Auth failures: any spike = credential stuffing in progress

Response times: establish baseline now — future degradation needs a baseline to compare

User actions: what are the first real users actually doing? (logging their paths)



\### 5.2 — Feedback Loop



Every week for the first month:

\[PRODUCT REVIEW]

Shipped: \[list]

Usage data: \[what users actually did vs. what we expected]

Top user complaint: \[the most frequent friction point]

Top user praise: \[what they actually value — often surprising]

Next iteration: \[one thing to improve based on evidence, not assumption]



The "top user complaint" field is the most valuable field in the whole document.

It is the only reliable signal about what to build next.



\### 5.3 — Technical Debt After Ship



Every performance compromise, known bug, or design shortcut made to ship:

→ T\[N] in tasks.md with label `\[DEBT]`, P0/P1/P2 severity, and a date commitment.

Debt items are reviewed every week. At least one P0 debt item is resolved per sprint.

Untracked debt is not "moving fast." It is losing ground to entropy.



\---



\## COMMANDER'S WORKFLOW CHEAT SHEET

New idea arrives:

→ BIAS-03 check: is a project currently in Phase 3? Park idea → reference/ideas.md

→ If no active project OR current project hit v1.0: proceed to Phase 0

Phase 0 (1-4h):

→ Write opportunity\_assessment.md

→ G1 + G2 + G3 gate: all yes → Phase 1

Phase 1 (4-48h):

→ Write technical\_blueprint.md

→ API contracts complete? → Phase 2

Phase 2 (1-4h):

→ Write tasks.md with T\[N] format, paired tests, 2.5x estimates

→ All tasks have acceptance criteria? → Phase 3

Phase 3 (execution):

→ One task → test → commit → repeat

→ Blocked? → \[BLOCK] protocol → unstuck in <30min

→ All tasks complete? → Phase 4

Phase 4 (code review + security + perf):

→ Load rules/coding.md + rules/security.md

→ Run checklists → fix P0/P1 → document P2/P3

→ Production readiness gate passes? → Phase 5

Phase 5 (ship):

→ Monitor 48h → weekly product review → iterate



\---



\## PROJECTS CURRENTLY TRACKED

\[ACTIVE]

minadoor-db (Pizzario CRM) — Phase 3, Tasks T8–T20 remaining

Horizon — architecture phase (Phase 1 status: unknown — needs blueprint)

GADA — Phase 0/1 (minister-level pitch done, technical blueprint incomplete)

atlantis-visa (openclaw) — Phase 3, active development

\[PARKED — return after first revenue]

Marcus/Superis system — ongoing parallel build

\[RULE] Only one project in Phase 3 at a time. Others can be in Phase 0-1 (planning).

This rule exists because execution context switching is the primary reason projects stall.



\---



\*"A plan that isn't written doesn't exist. A task without acceptance criteria isn't done.

A ship without monitoring isn't shipped."\*



Schema: 1.0.0 | Review trigger: after every project that either ships or fails.

Log what the workflow caught and what it missed. Evolve it from evidence.



