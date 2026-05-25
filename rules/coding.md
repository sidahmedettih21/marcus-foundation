\# Marcus Library — rules/coding.md

\# Complete Software Engineering Lifecycle — Repo to Production-Ready Saleable Product

\# Schema: 1.0.0 | Auto-loaded on: any coding, architecture, review, or debugging task



\---



\## PHASE 0 — CODEBASE ONBOARDING

\# Triggered when: new repo received, new project handed over, unfamiliar codebase.

\# Before writing a single line of code, Marcus runs this sequence.



\---



\### 0.1 — First Contact Checklist

\[ONBOARDING SEQUENCE]



Read README.md fully. Note: does it actually match the code?

Read existing tests (if any). Tests are the best documentation.

Map the dependency graph: what external services does this touch?

Run it locally. Does it work? Note every friction point.

Find the entry point. Trace one full request from HTTP in to DB and back.

Identify: what exists, what's missing, what's broken, what's a lie in the docs.

Write CODEBASE\_AUDIT.md before touching anything.





\### 0.2 — CODEBASE\_AUDIT.md Required Sections



\- \*\*Architecture summary\*\* (what it actually is, not what the README claims)

\- \*\*Tech stack\*\* with actual versions (not "latest")

\- \*\*Identified gaps\*\* — missing tests, missing error handling, missing migrations

\- \*\*Contract mismatches\*\* — frontend ↔ backend, API ↔ client

\- \*\*Security surface\*\* — auth mechanism, exposed endpoints, input validation status

\- \*\*Debt inventory\*\* — legacy patterns, deprecated APIs, workarounds in code

\- \*\*Confidence score\*\* (0.0–1.0): how well Marcus understands the system



\[!] Rule: No implementation begins until CODEBASE\_AUDIT.md exists and debt inventory is reviewed.

The reason: every hour spent on a codebase you don't understand compounds incorrectly.



\---



\## PHASE 1 — SPECIFICATION (No Code Zone)



\---



\### 1.1 — Opportunity Assessment (`opportunity\\\\\\\_assessment.md`)



Required sections, each with a completion test:



\*\*Problem\*\* — Who has it? (specific person, not "businesses"). How often? How painful on 1–10?

Completion test: Can Marcus describe the problem in one sentence from the user's perspective?



\*\*Solution\*\* — What are you building in exactly one sentence?

Completion test: A non-technical person understands what it does and doesn't do.



\*\*Target customer\*\* — Name them. Describe their day. What do they use right now?

Completion test: Marcus could write a cold email to this person and it wouldn't be generic.



\*\*Success criteria\*\* — Measurable. Time-bound. No vanity metrics.

Examples of acceptable: "100 paying users in 90 days", "€500 MRR by month 3"

Examples of rejected: "good user feedback", "people like it", "traction"



\*\*Risks\*\* — Top 3, each with a mitigation. Not "maybe the market is wrong." Specific.

Example risk: "TLScontact blocks automated sessions" → mitigation: CDP stealth mode + residential proxies.



\*\*Build vs. buy vs. ignore\*\* — For every major component: is there an OSS/SaaS alternative?

If build: justify why owning this is worth the maintenance burden.



\*\*Realistic timeline\*\* — Raw estimate + 2.5x multiplier applied. No raw estimates in plans.



\### 1.2 — Gate Condition for Phase 2



All sections of `opportunity\\\\\\\_assessment.md` complete AND reviewed → proceed.

Missing any section → blocked. No exceptions. This is the contract with future-Karl who will

inherit this codebase in 6 months under stress and needs to know why decisions were made.



\---



\## PHASE 2 — TECHNICAL BLUEPRINT (`technical\\\\\\\_blueprint.md`)



\---



\### 2.1 — Required Sections



\*\*Architecture diagram\*\* — ASCII is sufficient. Must show: all services, all data flows,

all external dependencies, all async boundaries. If you can't draw it, you don't understand it.



\*\*Data model\*\* — Every entity, every field, every relationship, every constraint.

\- Primary keys explicit (UUID vs. serial — justify the choice)

\- Foreign key constraints defined

\- Indexes specified (not "we'll add them later")

\- Multi-tenant marker explicit if applicable (which field is the tenant partition key?)

\- Enums defined (not free strings) for any field with bounded values



\*\*API contracts\*\* — For every endpoint, before any implementation:

METHOD /path

Auth: \[none | JWT | API key | session]

Rate limit: \[N req/min per user | per IP]

Request: { field: type (required/optional), constraints }

Response 2xx: { field: type }

Response 4xx: { code: string, message: string, details?: any }

Response 5xx: { code: string }

Side effects: \[what DB writes, what cache invalidations, what events emitted]



No endpoint exists without this contract. If the contract is missing, the endpoint doesn't exist yet.



\*\*Technology decisions with justifications\*\* — Every technology choice answered:

\- Why this, not the obvious alternative?

\- What's the migration path if this turns out to be wrong?



\*\*Deployment strategy\*\* — Where does this run? How does it get there? What's the rollback?

Not aspirational ("we'll use Kubernetes someday"). What runs this week?



\*\*Observability plan\*\* — What do you log? What do you alert on? How do you know it's broken

before a user tells you?



\### 2.2 — Gate Condition for Phase 3



API contracts written for all endpoints → tasks can be generated.

Without API contracts, you cannot write tests. Without tests, you cannot implement correctly.

This is not optional overhead. This IS the development.



\---



\## PHASE 3 — TASK BREAKDOWN (`tasks.md`)



\---



\### 3.1 — Task Anatomy



Every task in `tasks.md` must contain:

T\[N]: \[verb phrase describing what changes]

Acceptance criteria: \[done when X is true — testable, observable]

Files: \[files to create or modify]

Estimated hours: \[raw] → \[adjusted: raw × 2.5x]

Dependencies: \[T-IDs that must be complete first, or "none"]

Max lines: \[target — flag if >50, decompose if >80]



\### 3.2 — Task Rules



\- One task = one commit. No bundling.

\- Tasks are independent at execution time. Dependencies declared, never assumed.

\- No task touches more than 3 files. If it does: decompose.

\- Every task has a visible artifact (committed file, passing test, working endpoint).

&#x20; "Research X" is not a task. "Write decision to ADR-004 based on research" is a task.

\- Implementation tasks are always paired with test tasks. T8 (implement) → T8a (test).

&#x20; They can be the same task. They cannot both be missing.



\---



\## PHASE 4 — EXECUTION INVARIANTS



\---



\### 4.1 — Universal Code Rules (Language-Agnostic)



\*\*Strict typing, always.\*\*

Python: Pydantic models + mypy with `strict = true`. No `Any` without explicit comment justifying it.

TypeScript: `strict: true` in tsconfig. `any` is banned. `unknown` is acceptable with a type guard.

Rule: if the type system can catch it, let it. Type errors are free tests that run at zero cost.



\*\*No silent errors.\*\*

Every exception path is one of:

\- Caught and handled with logged context (what was the state when this happened?)

\- Caught and re-raised with added context (`raise SomeError("context") from original`)

\- Explicitly allowed to propagate (documented why in a comment)



Never: `except: pass`. Never: empty `catch {}`. Never: swallowed errors.

Silent errors are time bombs — they manifest at the worst possible moment with zero context.



\*\*Self-documenting code.\*\*

\- Docstrings answer WHY, not WHAT. The code answers WHAT.

\- Function names are verbs: `validate\\\\\\\_passport\\\\\\\_number()`, not `passport()`.

\- Variable names are nouns: `active\\\\\\\_clients`, not `data` or `result` or `temp`.

\- No abbreviations except universal ones: `id`, `url`, `db`, `req`, `res`, `ctx`, `err`.

\- Magic numbers are named constants. `MAX\\\\\\\_IMPORT\\\\\\\_BATCH\\\\\\\_SIZE = 1000`, not `1000` in code.



\*\*Single responsibility.\*\*

Functions do one thing. If the function name contains "and", it does two things → split it.

Files have one purpose. `utils.py` is a violation waiting to happen — name utilities by their domain.

Classes do not mix data access with business logic. Repositories for DB. Services for logic.



\*\*Immutability preference.\*\*

Default to immutable data structures. Mutate only when the alternative is O(n²) complexity.

Explicit mutation is documented. Surprise mutation is a bug.



\*\*Fail loud in development, fail gracefully in production.\*\*

In dev/test: assertions enabled, strict validation, verbose errors.

In production: structured error responses, no stack traces to clients, all errors logged internally.

This is controlled by environment, not by comment-toggling code.



\---



\### 4.2 — Code Generation Protocol (Marcus-Specific)



Before any code output, Marcus runs:

\[VERIFICATION]

Input spec: \[what the function/module receives]

Logic outline: \[algorithm or flow in plain English, before code]

Complexity: \[time and space if non-trivial]

Edge cases: \[minimum 3 — empty input, max input, invalid input]

Security surface: \[what could be injected, abused, or misused here?]

Ground truth: \[what does the test expect? write test expectation first]

Confidence: \[0.0–1.0 — if <0.85, flag \[?] and state uncertainty]



After code output, Marcus runs:

\[SELF-REVIEW]

□ Types: all inputs and outputs typed?

□ Errors: all exception paths handled or explicitly propagated?

□ Tests: does this code have a corresponding test?

□ Security: does this touch user input? Is it validated before use?

□ Side effects: does this write to DB/cache/external? Is that documented?



If any checkbox fails → fix before output. Not "I'll note this for later."



\---



\### 4.3 — Commit Discipline



Format: `type(scope): imperative description` — max 72 chars, present tense.



Types:

\- `feat` — new capability

\- `fix` — bug correction

\- `refactor` — restructure without behavior change

\- `test` — add or fix tests

\- `docs` — documentation only

\- `security` — security fix or hardening

\- `perf` — performance improvement

\- `chore` — tooling, dependencies, configuration

\- `migration` — database schema change



Rules:

\- Atomic commits: one logical change per commit. Reviewable in isolation.

\- No "wip", "stuff", "fixes", "updates" commit messages.

\- No committing commented-out code.

\- No committing `console.log` / `print()` debug statements.

\- No committing with failing tests. If you must (emergency): `\\\\\\\[!] FAILING TEST` prefix and a linked issue.



Branch strategy (solo developer):

\- `main` — production-ready always. If main is broken, you are on-call.

\- `dev` — integration branch. Features merge here first.

\- `feature/T\\\\\\\[N]-short-description` — one branch per task.

\- Delete feature branches after merge.



\---



\## PHASE 5 — TESTING



\---



\### 5.1 — Test Philosophy



Tests are the second user of your code. If it's hard to test, the design is wrong.

Tests are not a tax. They are a compiler for behavior. They are the documentation that

never lies — unlike comments, tests fail when they're wrong.



The goal is not coverage percentage. The goal is: can you confidently change this code

without manually re-testing everything? Yes → tests are working. No → tests are insufficient.



\### 5.2 — Test Pyramid



\*\*Unit tests (70%)\*\* — Fast. No I/O. No network. No DB. Mock everything external.

Test the logic, not the wiring. Input in, output out, assert the output.

Speed target: entire unit suite < 5 seconds. If slower, you're not unit testing.



\*\*Integration tests (20%)\*\* — Test the real DB, real Redis, real HTTP.

Use testcontainers or a local Docker Compose test environment.

Test the boundaries: does the endpoint actually return what the contract says?

Test the failure modes: what happens when the DB is down? When Redis is unavailable?



\*\*E2E tests (10%)\*\* — Critical user paths only. Full system. Slow. Run in CI, not dev loop.

Maximum 10 E2E tests per product. If you have more, you're testing implementation.



\### 5.3 — Test-First Generation (Non-Negotiable)



Write the test spec BEFORE implementation.

Test spec = given \[input], expect \[output], edge cases \[list].

Written in plain English first. Then as code.



Why: the act of writing the test forces you to answer "what does done look like?"

Without this answer, you cannot know when to stop implementing.



Marcus will not output implementation code for a function that doesn't have:

\- A corresponding test file path identified

\- At least 3 test cases specified (happy path, empty/null input, invalid input)



\### 5.4 — What to Test vs. What Not to Test



\*\*Test:\*\* behavior from the outside. What the function promises to do.

\*\*Test:\*\* error cases and edge cases.

\*\*Test:\*\* security-critical paths (auth, permissions, input validation) — 100% coverage required.

\*\*Test:\*\* any bug that was found in production — regression test before fix.



\*\*Don't test:\*\* implementation details (which internal function was called).

\*\*Don't test:\*\* the framework (FastAPI's routing works — Palantir doesn't test Pydantic).

\*\*Don't test:\*\* trivial getters/setters with no logic.

\*\*Don't test:\*\* third-party library behavior.



\### 5.5 — Coverage Gates



\- Overall: 80% line coverage minimum (enforced in CI — builds fail below this).

\- Auth/permissions: 100% (no exceptions — a missed auth test is a security incident waiting).

\- Payment/billing paths: 100%.

\- Import/export pipelines: 95% (data integrity is the product).

\- UI components: 60% minimum (test behavior, not rendering).



\### 5.6 — Test Data Discipline



\- No hardcoded test data. Factory functions that generate realistic, locale-appropriate data.

\- Use `faker` for synthetic data. Use real-format data (valid passport numbers, real date formats).

\- Test fixtures in `tests/fixtures/` — versioned, named by what they test.

\- Naming: `valid\\\\\\\_{scenario}.ext`, `invalid\\\\\\\_{reason}.ext`, `edge\\\\\\\_{case}.ext`

\- Test databases: isolated per test run. Never share state between tests.



\---



\## PHASE 6 — SECURITY



\---



\### 6.1 — STRIDE Before Every Feature



For every new feature surface (new endpoint, new input, new integration):



\*\*S — Spoofing:\*\* Can someone claim to be someone they're not? → Auth required?

\*\*T — Tampering:\*\* Can data be modified in transit or at rest without detection? → Integrity checks?

\*\*R — Repudiation:\*\* Can someone deny an action they took? → Audit log required?

\*\*I — Information Disclosure:\*\* Can someone access data they shouldn't? → Authorization check?

\*\*D — Denial of Service:\*\* Can someone exhaust this resource? → Rate limiting?

\*\*E — Elevation of Privilege:\*\* Can someone gain more access than granted? → Permission gates?



This takes 5 minutes. Not running it takes 5 weeks of incident response.



\### 6.2 — OWASP Top 10 (2025) — Mandatory Checklist Per Release



1\. \*\*Broken Access Control\*\* — every endpoint checks: is this user allowed to do this to this resource?

&#x20;  Not just "are they logged in." Are they allowed to access THIS specific record?

2\. \*\*Cryptographic Failures\*\* — no sensitive data in logs, URLs, or unencrypted storage.

&#x20;  Passwords: bcrypt/argon2 only. JWTs: RS256 (asymmetric). TLS everywhere.

3\. \*\*Injection\*\* — parameterized queries. Always. No string interpolation in SQL or shell.

&#x20;  User input is hostile until validated against a strict schema.

4\. \*\*Insecure Design\*\* — STRIDE was run before building. Security requirements exist before code.

5\. \*\*Security Misconfiguration\*\* — no default credentials. No exposed admin panels.

&#x20;  No debug mode in production. No stack traces to clients.

6\. \*\*Vulnerable and Outdated Components\*\* — `pip-audit` / `npm audit` in CI. No ignored vulnerabilities.

7\. \*\*Identification and Authentication Failures\*\* — JWT RS256, 15-min access tokens, rotating refresh.

&#x20;  Brute force protection on login. Account lockout after N failures.

8\. \*\*Software and Data Integrity Failures\*\* — signed releases. Dependency checksums verified.

&#x20;  No unverified data deserialization.

9\. \*\*Security Logging and Monitoring Failures\*\* — every auth failure logged.

&#x20;  Every permission denial logged. Logs are structured JSON, not human-readable strings.

10\. \*\*SSRF\*\* — no user-controlled URLs fetched by the server without allowlist validation.



\### 6.3 — Input Validation Rules



All user input is hostile. Default: reject. Explicitly allow.



Every API endpoint: validate against Pydantic schema before touching business logic.

String fields: max length enforced. No unbounded inputs.

File uploads: type validated by magic bytes, not extension. Size limit enforced.

Numeric fields: min/max enforced. No implicit integer overflow.

Enum fields: validated against closed list. No "accept anything and handle it later."

Date fields: explicit format. Reject ambiguous formats. Convert to UTC at boundary.

IDs: validate format (UUID structure) before DB query. Prevents injection via ID fields.





\### 6.4 — Secrets Management



Tiers of acceptable secret storage (in order of preference):



1\. \*\*Vault (HashiCorp, self-hosted)\*\* — production standard. Dynamic secrets, rotation, audit.

2\. \*\*Environment variables injected at runtime\*\* — acceptable for cloud deployments.

3\. \*\*.env file, gitignored, never committed\*\* — acceptable for local dev only.

4\. \*\*Anywhere else\*\* — not acceptable. Rotate immediately if discovered.



Pre-commit enforcement: `gitleaks` or `detect-secrets` runs on every commit attempt.

If a secret hits git history: rotate the secret FIRST, then scrub history.

Rotation schedule: API keys every 90 days, DB credentials every 180 days.



\### 6.5 — Authentication Standard

JWT RS256 (asymmetric, not HS256)

Access token: 15 minutes TTL

Refresh token: 7 days TTL, rotation on every use (old token invalidated)

Refresh token storage: httpOnly cookie (not localStorage, not sessionStorage)

Every endpoint: denied by default. Permission = explicit grant.

Permission check order: Authentication → Authorization → Rate limit → Business logic.

Never skip steps. Never combine them.

Rate limiting:

Per user: N requests/minute (varies by endpoint sensitivity)

Per IP: M requests/minute (prevents credential stuffing)

Per endpoint: specific limits for auth endpoints (login: 10/min. Refresh: 60/min.)



\---



\## PHASE 7 — CODE REVIEW



\---



\### 7.1 — Self-Review Protocol (Before Any PR / Before Committing)



Marcus runs this on every code output. Karl runs this before every commit.

\[CODE REVIEW — P0: SECURITY]

□ SQL injection: parameterized queries used everywhere?

□ Auth bypass: every endpoint checks authentication AND authorization?

□ Exposed secrets: no API keys, passwords, tokens in code or logs?

□ Input validation: all user inputs validated against strict schema before use?

□ IDOR: resource access checks use the requesting user's ID, not just the resource ID from the request?

\[CODE REVIEW — P1: CORRECTNESS]

□ Error handling: all exception paths handled or explicitly propagated?

□ Race conditions: any shared state that two concurrent requests could corrupt?

□ Edge cases: empty input, null values, max values handled?

□ Return values: all return paths return the correct type?

□ Async correctness: await on all async calls? No fire-and-forget without intent?

\[CODE REVIEW — P2: PERFORMANCE]

□ N+1 queries: any loop that triggers a DB query per iteration?

□ Missing indexes: queries on un-indexed columns in hot paths?

□ Blocking calls: synchronous I/O in async context?

□ Unbounded queries: SELECT without LIMIT on user-controlled collections?

□ Memory leaks: unclosed file handles, connections, or accumulated state?

\[CODE REVIEW — P3: MAINTAINABILITY]

□ Function length: >40 lines is a warning. >80 lines is a block.

□ Naming: functions are verbs, variables are nouns, names reveal intent?

□ Duplication: same logic appearing twice? Extract to shared function.

□ Tests: every new function has a corresponding test?

□ Comments: any comment that explains WHAT instead of WHY? Delete it.



Severity enforcement:

\- P0 failures: block. Do not ship. Fix now.

\- P1 failures: block unless documented exception approved.

\- P2/P3 failures: log as tech debt. Ship with tracking issue created.



\---



\## PHASE 8 — PERFORMANCE



\---



\### 8.1 — Performance Philosophy



Measure before optimizing. Intuition about bottlenecks is wrong 80% of the time.

The only valid optimization is one that addresses a measured problem with a measured improvement.



\### 8.2 — Baseline Targets (Web API)

p50 response time: <100ms for read endpoints, <300ms for write endpoints

p99 response time: <500ms for read, <1s for write

Error rate: <0.1% for any endpoint in production

DB query time: <50ms for 95th percentile of queries



If a baseline target is missed: profile before refactoring. Add an index before rewriting the query.

Rewrite the query before changing the schema. Change the schema before changing the architecture.



\### 8.3 — Database Performance Rules



\- Every foreign key has an index. Every column in a WHERE clause on a hot path has an index.

\- EXPLAIN ANALYZE before adding any index. Indexes have write cost.

\- Multi-tenant tables: `tenant\\\\\\\_id` leads every composite index.

\- Connection pooling: pgbouncer in front of Postgres for any production workload.

\- Pool size formula: `(cpu\\\\\\\_cores × 2) + effective\\\\\\\_spindles` — don't guess.

\- Migrations run during low-traffic windows. Long-running migrations use zero-downtime patterns.



\### 8.4 — Caching Strategy

Cache what: expensive computations, slow external API responses, frequently-read rarely-changed data.

Cache where: Redis with explicit TTL. No infinite TTL. No implicit caching.

Cache invalidation: event-driven (write through), not time-based (TTL-only).

Cache key format: {service}:{entity}:{id}:{version} — prevents key collision across services.

Never cache: security-sensitive data (permissions, auth tokens). User-specific data in shared cache.



\---



\## PHASE 9 — OBSERVABILITY



\---



\### 9.1 — Logging Standard



Structured JSON logs. Every log line contains:

```json

{

\\\&#x20; "timestamp": "ISO8601",

\\\&#x20; "level": "INFO|WARNING|ERROR|CRITICAL",

\\\&#x20; "request\\\\\\\_id": "uuid — injected by middleware, traces a request through the system",

\\\&#x20; "user\\\\\\\_id": "uuid or null",

\\\&#x20; "service": "service-name",

\\\&#x20; "event": "what\\\\\\\_happened",

\\\&#x20; "duration\\\\\\\_ms": 0,

\\\&#x20; "error": "error message or null",

\\\&#x20; "context": {}

}

```



No `print()` in production. No `logger.info("Starting the thing")`. Log events with meaning.

Good: `logger.info("client\\\\\\\_import\\\\\\\_completed", extra={"imported": 47, "skipped": 3, "duration\\\\\\\_ms": 234})`

Bad: `logger.info("Import done")`



\### 9.2 — What to Alert On

Alert immediately (PagerDuty/Telegram):



Error rate >1% on any endpoint for >2 minutes

p99 latency >2s for >5 minutes

Any 5xx on auth endpoints (login, refresh, logout)

Any failed DB migration

Any secret rotation failure

Service health check failing



Alert in next business day (digest):



Error rate >0.1% sustained for >30 minutes

Unusual traffic patterns (10x baseline on any endpoint)

Dependency version with known CVE detected





\### 9.3 — Health Check Standard



Every service exposes `GET /health`:

```json

{

\\\&#x20; "status": "healthy|degraded|unhealthy",

\\\&#x20; "version": "semver",

\\\&#x20; "uptime\\\\\\\_seconds": 0,

\\\&#x20; "dependencies": {

\\\&#x20;   "database": "healthy|unhealthy",

\\\&#x20;   "redis": "healthy|unhealthy",

\\\&#x20;   "external\\\\\\\_api": "healthy|unhealthy"

\\\&#x20; }

}

```



`degraded` = some dependencies slow but core function works.

`unhealthy` = cannot serve requests.

Load balancers and orchestrators use this. If it doesn't exist, outages are invisible until users complain.



\---



\## PHASE 10 — CI/CD PIPELINE



\---



\### 10.1 — CI Pipeline (Runs on Every Push)

Stage 1 — Fast Feedback (must complete in <3 minutes):



Dependency install (cached)

Type checking (mypy / tsc)

Linting (ruff / ESLint)

Secret scanning (gitleaks)

Unit tests



Stage 2 — Integration (must complete in <10 minutes):



Integration tests (testcontainers)

Security scan (pip-audit / npm audit)

Coverage gate (fail if below threshold)



Stage 3 — Quality (must complete in <20 minutes):



E2E tests (against staging environment)

Performance regression check (if >20% latency increase on key endpoints, fail)

Build artifact (Docker image, tagged with commit SHA)





Rule: if any stage fails, the branch cannot merge. No manual override without two approvals

(even for solo developer — be the second approval after sleeping on it).



\### 10.2 — CD Pipeline (Deploys from main)

main merge → CI passes → Docker image built + pushed to registry

→ Deploy to staging (automatic)

→ Smoke tests on staging (automatic)

→ Manual promotion gate (Karl approves prod deploy)

→ Deploy to production (canary: 10% traffic for 10 minutes)

→ Automated rollback if error rate spikes

→ Full rollout if healthy



Every production deploy has a rollback path. "Deploy and pray" is not a deployment strategy.



\---



\## PHASE 11 — RELEASE MANAGEMENT



\---



\### 11.1 — Semantic Versioning



`MAJOR.MINOR.PATCH`

\- PATCH: bug fixes, security patches, no behavior change

\- MINOR: new features, backward-compatible

\- MAJOR: breaking change in API contract or data schema



Every release tagged in git. Every release has a CHANGELOG entry.



\### 11.2 — CHANGELOG Format



```markdown

\\\\## \\\\\\\[1.2.0] — 2026-05-25



\\\\### Added

\\\\- Client import preview endpoint with row-level validation



\\\\### Fixed

\\\\- Passport duplicate detection now checks intra-batch duplicates



\\\\### Security

\\\\- Refresh token rotation enforced on every use



\\\\### Breaking Changes

\\\\- Import confirm endpoint now accepts body instead of query param

```



\### 11.3 — Feature Flags



New features behind flags until validated. Never ship an experiment directly to all users.

Flag states: `off` (default), `on` (enabled for all), `partial` (enabled for % or specific users).

Dead flags (no longer needed) are removed within 30 days of full rollout. Flag debt is real debt.



\---



\## PHASE 12 — PRODUCTION READINESS CHECKLIST



\# Before any product is sold, subscribed to, or given to a real user:

\[PRODUCTION READINESS GATE]

Infrastructure:

□ TLS termination on all endpoints (no HTTP in production)

□ Rate limiting active on all auth endpoints

□ Secrets in environment variables (not in code, not in files committed to git)

□ Database backups configured and tested (restore, not just backup)

□ Health check endpoint live and monitored

Security:

□ OWASP Top 10 checklist reviewed for this release

□ STRIDE done for all new feature surfaces

□ No known CVEs with CVSS >7 in dependencies (pip-audit / npm audit clean)

□ Auth tokens short-lived, refresh rotation active

□ All user inputs validated

Observability:

□ Structured logging active

□ Error alerting configured (Telegram minimum)

□ Health check endpoint monitored externally

□ At least one dashboard showing: error rate, latency, active users

Reliability:

□ Service restart policy configured (always restart on crash)

□ Database connection pooling active

□ Circuit breakers on external dependencies

□ Rollback procedure documented and tested

Operations:

□ Runbook exists: how to restart, how to rollback, how to check logs

□ Known incident types documented: what are the top 3 most likely failures?

□ On-call alert configured (even if it's just Telegram to Karl's phone)

Data:

□ Database migrations tested on production-sized data (or representative sample)

□ No irreversible migrations without data backup taken immediately before

□ User data deletion path exists (regulatory compliance — even in Algeria)



If any item is unchecked: document why it's acceptable risk, or fix it.

"We'll do it later" is not a risk assessment. It is a future incident.



\---



\## PHASE 13 — POST-SHIP



\---



\### 13.1 — First 48 Hours



Watch these metrics continuously:

\- Error rate on all endpoints (target: <0.1%)

\- Auth endpoint performance (login, refresh — first sign of real-world edge cases)

\- DB query performance under real load

\- Any unexpected 5xx responses → investigate immediately, do not suppress alerts



\### 13.2 — Technical Debt Protocol



Every piece of tech debt is a logged issue with:

\- Description of the shortcut taken

\- Why it was acceptable at the time

\- What breaks if this debt is not paid

\- Priority: P0 (pay in current sprint) / P1 (pay in next sprint) / P2 (pay before scale)



Weekly: review tech debt log. Kill at least one P0 before adding features.

Rule: technical debt is not shameful. Untracked technical debt is negligence.



\### 13.3 — Post-Mortem Protocol (Any Production Incident)



Triggered by: any P0 incident, any data loss, any auth failure in production.



Required within 48 hours:

INCIDENT: \[title]

Date/time: \[when]

Duration: \[how long affected]

Impact: \[who was affected, how many users, what they lost]

Timeline: \[what happened, when, in chronological order]

Root cause: \[the actual cause, not symptoms]

Contributing factors: \[what made this possible]

Resolution: \[what fixed it]

Prevention: \[what changes prevent recurrence]

Action items: \[specific tasks, owners, deadlines]



Blame-free. The goal is system improvement, not fault assignment.

A post-mortem that finds "human error" as the root cause is an incomplete post-mortem.

Human error is always a symptom of a system that made the error too easy to make.



\---



\## ARCHITECTURAL DECISION RECORDS (ADRs)



Every significant architectural choice gets an ADR in `docs/decisions/ADR-NNN.md`:



```markdown

\\\\# ADR-001: JWT RS256 over HS256



\\\\## Status: Accepted



\\\\## Context

Need stateless auth. HS256 requires sharing the secret with any service that validates tokens.

RS256 allows public key distribution — services can verify without the signing key.



\\\\## Decision

Use RS256 with 15-minute access tokens and rotating refresh tokens in httpOnly cookies.



\\\\## Consequences

\\\\- Positive: token validation does not require secret distribution

\\\\- Positive: compromised verification service cannot forge tokens

\\\\- Negative: key rotation is more complex (requires distributing new public key)

\\\\- Negative: slightly larger token size

```



ADRs are never deleted. Superseded ADRs are marked "Superseded by ADR-NNN."

Future-Karl needs to know why past-Karl made this choice, especially when it seems wrong.



\---



\*"Software is not done when it works. It is done when it cannot easily be broken, cannot easily be

misunderstood, and can be handed to the next person — including future you — without a phone call."\*



Schema: 1.0.0 | Review trigger: major stack change or new language added to the project.



