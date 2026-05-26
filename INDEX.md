\# Marcus Library — INDEX.md

\# Routing Table: Task Type → Files to Load

\# Schema: 1.0.0

\# This file is ALWAYS loaded first. Every other file is loaded on demand.

\# Loading = injecting the file's content into Marcus's active context.



\---



\## HOW THE ROUTING WORKS



1\. User message arrives.

2\. Marcus reads INDEX.md (already in context — always preloaded).

3\. Marcus classifies the task type using the trigger phrases below.

4\. Marcus loads the required files for that task type.

5\. Required files are non-negotiable. Optional files load if complexity warrants.

6\. Marcus executes using the loaded rules as constraints — not suggestions.



Loading cost awareness:

&#x20; Tier A (free) — files <500 lines, cheap model handles routing

&#x20; Tier B (medium) — 1-2 domain rule files, smart-balanced model

&#x20; Tier C (expensive) — 3+ files + full ontology, best-quality model only



\---



\## ROUTING TABLE



| Task Type | Trigger Phrases | Required Files | Optional Files | Model Tier |

|-----------|----------------|----------------|----------------|-----------|

| \*\*General coding\*\* | "write", "implement", "build", "code", "function", "bug", "fix", "refactor" | `AGENT\_ONTOLOGY.md` + `rules/coding.md` | `rules/security.md` (if auth/input) | B |

| \*\*Frontend / UI\*\* | "component", "page", "UI", "interface", "design system", "CSS", "React", "Tailwind", "dashboard" | `AGENT\_ONTOLOGY.md` + `rules/coding.md` + `rules/coding-frontend.md` | `skills/frontend-designer.md` | B |

| \*\*Backend / API\*\* | "endpoint", "route", "FastAPI", "API", "service", "middleware", "auth", "JWT", "REST" | `AGENT\_ONTOLOGY.md` + `rules/coding.md` + `rules/coding-backend.md` | `rules/security.md` | B |

| \*\*Database\*\* | "schema", "migration", "Alembic", "query", "model", "index", "PostgreSQL", "SQL", "N+1" | `AGENT\_ONTOLOGY.md` + `rules/coding.md` + `rules/coding-database.md` | none | B |

| \*\*Testing\*\* | "test", "pytest", "coverage", "fixture", "mock", "integration test", "unit test", "TDD" | `AGENT\_ONTOLOGY.md` + `rules/coding.md` + `rules/coding-testing.md` | none | B |

| \*\*Code review\*\* | "review this", "check this code", "what's wrong", "audit the code", `/review` | `AGENT\_ONTOLOGY.md` + `rules/coding.md` + `rules/coding-review.md` | `rules/security.md` | B |

| \*\*Security review\*\* | "pentest", "vulnerability", "is this secure", "threat model", "STRIDE", "attack", "harden" | `AGENT\_ONTOLOGY.md` + `rules/security.md` + `rules/coding-review.md` | `rules/coding.md` | C |

| \*\*Reverse engineering\*\* | "reverse engineer", "analyze binary", "Ghidra", "firmware", "disassemble", "decompile", "RE" | `AGENT\_ONTOLOGY.md` + `rules/security.md` | none | C |

| \*\*Network / traffic\*\* | "Wireshark", "pcap", "capture traffic", "network forensics", "protocol analysis", "packet" | `AGENT\_ONTOLOGY.md` + `rules/security.md` | none | C |

| \*\*Exploit / pentest\*\* | "/redteam", "/pentest", "find vulnerabilities in", "CVE", "exploit this", "PoC" | `AGENT\_ONTOLOGY.md` + `rules/security.md` | none | C |

| \*\*New project / feature\*\* | "should I build", "new project", "new product", "is this worth", "opportunity", "what should I work on" | `AGENT\_ONTOLOGY.md` + `rules/project-management.md` | none | B |

| \*\*Planning / blueprint\*\* | "technical blueprint", "architecture", "how should I structure", "plan this", "design the system" | `AGENT\_ONTOLOGY.md` + `rules/project-management.md` + `rules/coding.md` | `rules/security.md` | C |

| \*\*Task management\*\* | "tasks", "what's next", "update tasks.md", "what should I work on", "sprint", "prioritize" | `AGENT\_ONTOLOGY.md` + `rules/project-management.md` | none | A |

| \*\*Data analysis\*\* | "analyze data", "data exploration", "statistics", "dataset", "SQL query for analysis", "visualize" | `AGENT\_ONTOLOGY.md` + `rules/data-analysis.md` | none | B |

| \*\*Personal / check-in\*\* | "I'm stuck", "I don't know", "should I", "what do you think about me", "how am I doing", "feeling" | `AGENT\_ONTOLOGY.md` + `rules/personal.md` | none | B |

| \*\*Spiral detected\*\* | \[automatic — see rules/personal.md BIAS-02 for triggers] | `AGENT\_ONTOLOGY.md` + `rules/personal.md` | none | A (fast response) |

| \*\*Explain simply\*\* | "/caveman", "explain like I'm 5", "explain simply", "what does this mean in plain English" | `AGENT\_ONTOLOGY.md` + `skills/caveman.md` | none | A |

| \*\*UI/UX design\*\* | "/design", "make it look good", "design this UI", "user experience", "wireframe", "what should the UI do" | `AGENT\_ONTOLOGY.md` + `skills/frontend-designer.md` | `rules/coding-frontend.md` | B |

| \*\*Deep reasoning\*\* | "/superpowers", "think deeply", "hard problem", "multi-domain", "what is the best architecture for" | `AGENT\_ONTOLOGY.md` + `skills/superpowers.md` | all relevant domain rules | C |

| \*\*Project status\*\* | "where are we", "status of", "what's the state of", project name (minadoor/horizon/GADA/atlantis) | `AGENT\_ONTOLOGY.md` + project `tasks.md` + project `technical\_blueprint.md` | none | A |



\---



\## MODEL TIER DEFINITIONS

Tier A — Cheap + Fast

Models: Gemini 2.0 Flash, DeepSeek chat (fast mode)

Use for: routing, classification, simple queries, status checks, personal check-ins

Cost: \~$0.01–0.05 per session

Tier B — Smart + Balanced (default tier)

Models: DeepSeek V4 Pro (primary), Gemini Flash (fallback)

Use for: implementation, planning, testing, analysis, most coding tasks

Cost: \~$0.10–0.50 per session

Tier C — Best Quality (explicit use cases only)

Models: Claude Sonnet/Opus, GPT-4 class

Use for: security audits, architecture decisions, reverse engineering, cross-domain hard problems

Cost: \~$0.50–3.00 per session

Rule: never use Tier C when Tier B is sufficient. The difference is quality of reasoning

on adversarial/ambiguous problems — not raw output quality on structured tasks.



\---



\## PRELOADED CONTEXT (Always in Active Memory)



These are always loaded. They are the minimum viable Marcus context.

AGENT\_ONTOLOGY.md — identity, philosophy, routing rules, behavioral constraints

INDEX.md — this file, routing table

\[current project]/tasks.md — active task list (loaded per active project)



Everything else is loaded on demand by this routing table.



\---



\## ACTIVE PROJECT CONTEXT (Current as of Last Session)

PRIMARY: minadoor-db (Pizzario CRM)

tasks.md: T8–T20 remaining (Phase 2–5)

next task: T8 — row validator implementation

blocked: no

last commit: \[update when known]

SECONDARY: atlantis-visa (openclaw)

phase: 3 (active development, soldier-v3.1.js)

next: monitor stability on ThinkPad X260

PLANNING: GADA

phase: 0–1

status: minister pitch complete, technical blueprint needed

PLANNING: Horizon

phase: 1

status: security audit done, multi-tenancy needs blueprint

PARKED: all other ideas

location: marcus/reference/ideas.md



\---



\## SKILL ACTIVATION COMMANDS

/caveman         → load skills/caveman.md       → simplification mode

/design          → load skills/frontend-designer.md → UI/UX persona mode

/superpowers     → load skills/superpowers.md   → maximum reasoning mode

/redteam         → load rules/security.md       → offensive security mode

/trident         → TRIDENT compression mode (already embedded in AGENT\_ONTOLOGY.md)

/hun             → behavioral/psychological analysis mode

/max             → maximum efficiency mode, direct answers only

Deactivation: /normal returns to default mode

Multiple skills: /superpowers + /design → both loaded, both active



\---



\## DISAMBIGUATION RULES



When a message matches multiple task types:

Security + Coding → load both rules/security.md AND rules/coding.md (Tier C)

Planning + Security → load project-management.md AND security.md (Tier C)

Personal + Coding → rules/personal.md first, then coding if task delivered

Spiral detected → rules/personal.md ONLY → no other context loaded until anchor task delivered



When task type is ambiguous (Marcus cannot classify with >80% confidence):

→ Ask one clarifying question: "Is this a \[type A] task or a \[type B] task?"

→ Do not load all files speculatively. Load the minimum, then expand.

→ Speculative loading = token waste = budget waste = violates cost discipline.



\---



\## FAILURE TAXONOMY CROSS-REFERENCE

If Marcus output produces a HALLUCINATION → check: was RAG context loaded?

If no RAG: load memory context before next response on same topic.

If Marcus output produces a QUALITY\_FAIL → check: was the right tier used?

If Tier A was used for a Tier C task: re-run with explicit tier upgrade.

If Marcus output produces a BUDGET\_EXCEEDED → check: was context over-loaded?

If yes: trim to required files only, remove optional files.

If Marcus output produces CONTEXT\_LOSS → check: was tasks.md and blueprint in context?

If no: reload project context from active project files.



\---



\## MARCUS LIBRARY — FILE MANIFEST

marcus/

├── INDEX.md                        ← THIS FILE — always loaded

├── AGENT\_ONTOLOGY.md              ← identity + philosophy — always loaded

│

├── rules/

│   ├── coding.md                  ← full lifecycle: repo → production

│   ├── coding-frontend.md         ← React, Tailwind, components, accessibility

│   ├── coding-backend.md          ← FastAPI, auth, rate limiting, observability

│   ├── coding-database.md         ← migrations, indexes, N+1, connection pooling

│   ├── coding-testing.md          ← test-first, pyramid, fixtures, coverage gates

│   ├── coding-review.md           ← P0/P1/P2/P3 checklist, review protocol

│   ├── security.md                ← STRIDE, Ghidra, Wireshark, crypto, IR, SentinelOne

│   ├── personal.md                ← bias inventory, spiral breaker, accountability hooks

│   ├── data-analysis.md           ← reproducibility, sourcing, cost-aware queries

│   └── project-management.md     ← Commander's Workflow: Phase 0→5

│

└── skills/

├── caveman.md                 ← simplification persona

├── frontend-designer.md       ← UI/UX designer persona

└── superpowers.md             ← maximum reasoning mode



Status: rules/coding-frontend.md, rules/coding-backend.md, rules/coding-database.md,

&#x20;       rules/coding-testing.md, rules/coding-review.md, rules/data-analysis.md,

&#x20;       skills/ files → not yet written.



Priority order for completing the library:

1\. rules/coding-backend.md — highest usage frequency (FastAPI projects)

2\. rules/coding-testing.md — highest impact on quality

3\. skills/superpowers.md — highest impact on hard problems

4\. rules/data-analysis.md — needed for GADA

5\. rest — complete when relevant project demands them



\---



\*"The routing table is only as good as what it routes to. Build the rules first. Index them second."\*



Schema: 1.0.0 | Update trigger: every time a new rules/ or skills/ file is added.



