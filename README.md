# AI Development OS

A human-controlled framework for safe, structured, AI-assisted software development using Claude Code.

---

## Short description

AI Development OS is a global rule and memory system that lives outside any single project. It defines how Claude Code should behave across every project under `~/Projects` — what roles it can take, how it handles memory between sessions, what it needs human approval for, and how it fits into a real Git/GitHub workflow.

It is not a fully autonomous agent. It is a framework that makes AI-assisted development safer, more repeatable, and easier to pick up after a break.

---

## Why this exists

Working with Claude Code across multiple projects without a shared structure creates recurring problems:

- Every session starts cold — no memory of what was decided, what branch is active, what the risks are.
- Without role boundaries, the AI tries to plan, build, test, and deploy all at once.
- Safety rules (don't touch production data, don't edit `.env`, don't deploy without approval) have to be re-stated in every prompt.
- Git discipline — branch naming, commit conventions, PR format — drifts unless it's embedded in the rules.
- There's no clean answer to "pick up where we left off."

This framework solves those problems once, globally, so they don't need solving per project.

---

## What it does

- Defines a **master framework** with core safety rules, a development workflow, and a human approval system.
- Provides **specialist rule files** for coding standards, security, deployment, and memory — loaded only when relevant.
- Provides **agent roles** so planning, building, testing, security review, and deployment review are separated rather than blurred.
- Provides **project memory files** so context (status, decisions, backlog, summaries) survives between sessions.
- Provides **templates** so any new project can be connected to the framework in minutes.
- Enforces **human approval gates** before anything irreversible: commits, deployments, migrations, production changes.

---

## Core components

### Specialist rule files

| File | Purpose |
|------|---------|
| [`AUTONOMOUS_DEV_FRAMEWORK.md`](./AUTONOMOUS_DEV_FRAMEWORK.md) | Master index. Core safety rules, workflow, orchestration, AI authority levels, output format. The first file every session reads. |
| [`AI_AGENT_RULES.md`](./AI_AGENT_RULES.md) | 8 agent roles with missions, activation conditions, outputs, and handoffs. |
| [`GLOBAL_CODING_STANDARDS.md`](./GLOBAL_CODING_STANDARDS.md) | Code quality: architecture boundaries, safe refactoring, API compatibility, config discipline, logging, performance, generated code, developer handoff. |
| [`SECURITY_RULES.md`](./SECURITY_RULES.md) | Secrets, production data, input validation, external APIs/webhooks, file uploads, infrastructure, AI-specific security, severity levels. |
| [`DEPLOYMENT_RULES.md`](./DEPLOYMENT_RULES.md) | Deployment authority, branching, commit gates, migration safety, CI/CD, pre/post-deploy checklists, failed deployment response, rollback. |
| [`MEMORY_SYSTEM.md`](./MEMORY_SYSTEM.md) | Memory authority order, per-project memory files, lightweight Agile vocabulary, session start/end routines. |
| [`CONTEXT_MANAGEMENT_RULES.md`](./CONTEXT_MANAGEMENT_RULES.md) | Context hygiene, batch discipline, minimal filesystem access, and prevention of unnecessary repository scans or 1M context usage during routine tasks. |

### Project templates

| Template | Purpose |
|----------|---------|
| [`PROJECT_CLAUDE_TEMPLATE.md`](./PROJECT_CLAUDE_TEMPLATE.md) | Copy into a project as `CLAUDE.md`. Points back to this framework. Holds stack, commands, AI authority level, do-not-touch list. |
| [`PROJECT_ONBOARDING_TEMPLATE.md`](./PROJECT_ONBOARDING_TEMPLATE.md) | Run once when entering a new/unknown project. Covers stack detection, risk scan, memory file check, Agile context. |
| [`PROJECT_STATUS_TEMPLATE.md`](./PROJECT_STATUS_TEMPLATE.md) | Copy into a project as `PROJECT_STATUS.md`. Updated at the start and end of every session. |

---

## Agent roles

The framework uses 8 roles, loosely inspired by BMAD-style agent discipline. Each role has a defined mission, activation condition, output, and handoff to the next role. Roles don't blur — an Analyst doesn't write code; a Developer doesn't plan architecture.

| # | Role | When to use |
|---|------|------------|
| 1 | **Strategic Advisor** | New feature, direction change, prioritisation, comparing approaches. |
| 2 | **Analyst** | Unclear scope, entering a new area, inspecting before changing. |
| 3 | **Product Manager** | Turning a problem into a concrete, testable spec. |
| 4 | **Architect** | Cross-module changes, data model changes, new dependencies. |
| 5 | **Developer** | Agreed plan exists, feature branch is ready. |
| 6 | **QA / Test Architect** | Code is written, before commit — run and write tests. |
| 7 | **Security Reviewer** | Before commit/PR — always for auth, secrets, data, APIs, infra. |
| 8 | **DevOps / Deployment Reviewer** | Preparing a commit, PR, or release. |

**Shortened flows** are allowed for low-risk tasks:
- `Developer → QA` for trivial, well-understood changes.
- `Analyst → Developer → QA` for small changes needing a quick look first.

**Security Reviewer** can be activated early (before Developer) when the task touches auth, permissions, payments, secrets, or external APIs.

Full role definitions, including handoffs and outputs, are in [`AI_AGENT_RULES.md`](./AI_AGENT_RULES.md).

---

## Lightweight Agile layer

The framework includes a minimal Agile vocabulary for tracking work across sessions — no sprints, no ceremonies, no story points unless the project has multiple humans who need them.

| Concept | Meaning |
|---------|---------|
| **Epic** | A large outcome being worked toward. |
| **Story** | A small, executable task. |
| **Acceptance criteria** | How we know the story is done. |
| **Backlog** | Prioritised list of work. |
| **Retrospective** | Learning captured after major milestones. |

Every next task is defined with: Action, Why it matters, Done when.

---

## Project memory system

Per-project memory files live in the project root and are updated at the end of every session.

| File | Purpose |
|------|---------|
| `PROJECT_STATUS.md` | Current phase, branch, last/next task, risks, test/deploy status, AI authority level. |
| `PROJECT_SUMMARY.md` | Compressed one-page overview for fast context recovery on large projects. |
| `PROJECT_BACKLOG.md` | Epics, stories, priorities, acceptance criteria. |
| `DECISION_LOG.md` | Major decisions, rationale, alternatives considered, date. |
| `RETROSPECTIVE.md` | What worked, what failed, what to change — written after major milestones. |

**Session start:** read `CLAUDE.md` → `PROJECT_STATUS.md` → optionally `PROJECT_SUMMARY.md`, `PROJECT_BACKLOG.md`, `DECISION_LOG.md`.

**Session end:** update `PROJECT_STATUS.md` always; update other files if decisions were made, priorities changed, or a milestone was reached.

Memory authority order (highest wins when sources conflict):
1. User instruction in the current session
2. Project `CLAUDE.md`
3. `PROJECT_STATUS.md`
4. `DECISION_LOG.md`
5. README / docs
6. Global `_AI_RULES`

---

## Safety and human approval gates

The framework has three layers of safety:

**1. AI authority levels** (set per project in `CLAUDE.md` or `PROJECT_STATUS.md`):

| Level | What Claude may do |
|-------|-------------------|
| `read-only` | Inspect and report only. |
| `propose-only` | Suggest changes; do not apply them. |
| `may-edit` | Edit files on a feature branch. No commits without approval. |
| `may-commit` | Edit and commit. No deployment without approval. |
| `may-deploy-with-approval` | May deploy after explicit human confirmation in the current session. |

Production deployment always requires explicit human approval in the current session, regardless of authority level.

**2. Safety gates** — stop and provide risk assessment + rollback plan before changing anything in:
authentication, authorization, payments, database migrations, production deployment, user data, production data, file uploads, external APIs/webhooks, security policies, permissions, infrastructure, environment variables, secrets, or AI/prompt-injection-sensitive workflows.

**3. Absolute DO NOT list** — enforced regardless of instruction:
- Never push to `main`/`master` directly.
- Never deploy to production automatically.
- Never delete databases, uploads, backups, or user data.
- Never edit `.env` files or expose secrets.
- Never rewrite the app from scratch without justification.
- Never hide uncertainty.

---

## How to connect a new project

1. Copy [`PROJECT_CLAUDE_TEMPLATE.md`](./PROJECT_CLAUDE_TEMPLATE.md) into the project root as `CLAUDE.md` and fill the placeholders.
2. Run [`PROJECT_ONBOARDING_TEMPLATE.md`](./PROJECT_ONBOARDING_TEMPLATE.md) as an Analyst task to inspect the stack, commands, conventions, and risks.
3. Copy [`PROJECT_STATUS_TEMPLATE.md`](./PROJECT_STATUS_TEMPLATE.md) into the project root as `PROJECT_STATUS.md`.
4. Create the other memory files as needed: `PROJECT_SUMMARY.md`, `PROJECT_BACKLOG.md`, `DECISION_LOG.md`.
5. In each Claude Code session, start by reading `CLAUDE.md` → `PROJECT_STATUS.md`.

---

## Validation status

**First validation project:** LegionTrap TI (`~/Projects/gitrepo/legiontrap-ti`) — 2026-05-30.

### Confirmed working
- Project onboarding using the framework's templates.
- Memory file creation: `CLAUDE.md`, `PROJECT_STATUS.md`, `PROJECT_SUMMARY.md`, `PROJECT_BACKLOG.md`, `DECISION_LOG.md`.
- Analyst role: inspecting `.bak` files, temp files, `node_modules`, `.gitignore`, test event files.
- Developer role: removing tracked `.bak` files from Git, fixing `.gitignore` conflict residue.
- Cleanup task: removing `tmp_events_test.jsonl` via a separate approved step.
- Full PR workflow: `docs/` and `chore/` branches → commits → GitHub PRs → merges into main → branch deletion.
- Human approval at every stage. No autonomous commits or deployments.

### Still needs validation
- Strategic Advisor, Architect, QA/Test Architect, Security Reviewer, DevOps Reviewer roles.
- Feature development, database migrations, deployment pipeline changes.
- Multi-session memory recovery using `PROJECT_SUMMARY.md`.
- `DECISION_LOG.md` under a real contested architectural decision.
- `RETROSPECTIVE.md` after a major milestone.
- `may-deploy-with-approval` authority level in a real deployment.
- Conflict resolution between memory sources.

See [`FRAMEWORK_VALIDATION_LOG.md`](./FRAMEWORK_VALIDATION_LOG.md) for the full validation record.

---

## Current limitations

- The framework has been validated on low-risk tasks only. Feature development and deployment flows are not yet tested.
- Memory files must be updated manually at session end — there is no automated sync.
- Roles are enforced by instruction, not by technical isolation. Discipline depends on following the rules.
- The Agile layer is lightweight by design — not suitable for teams without adaptation.
- Templates require manual copying and filling per project.

---

## Repository structure

```
_AI_RULES/
│
├── AUTONOMOUS_DEV_FRAMEWORK.md   ← Master index. Start here.
├── AI_AGENT_RULES.md             ← Agent roles and pipeline
├── GLOBAL_CODING_STANDARDS.md    ← Code quality rules
├── SECURITY_RULES.md             ← Security rules and checklist
├── DEPLOYMENT_RULES.md           ← Deployment authority and gates
├── MEMORY_SYSTEM.md              ← Memory files and session routines
│
├── PROJECT_CLAUDE_TEMPLATE.md    ← Template: project CLAUDE.md
├── PROJECT_ONBOARDING_TEMPLATE.md ← Template: first-session inspection
├── PROJECT_STATUS_TEMPLATE.md    ← Template: living status file
│
├── FRAMEWORK_VALIDATION_LOG.md   ← Validation history
├── PORTFOLIO_PROJECT_BRIEF.md    ← Portfolio story and website content
├── README.md                     ← This file
│
└── _backups/                     ← Timestamped backups before edits
```

---

## Portfolio note

This project is documented as a portfolio piece in [`PORTFOLIO_PROJECT_BRIEF.md`](./PORTFOLIO_PROJECT_BRIEF.md). It demonstrates:

- Systematic thinking about AI tooling and safe human-AI collaboration.
- Software engineering process: branching, commit discipline, PR workflow, code review gates.
- Security awareness: structured handling of secrets, production data, deployment authority.
- Technical writing: precise, operational documentation that is actively used.
- Iterative development: built and refined through real use, not written once and shelved.

The project began from a personal need to work more safely and consistently with Claude Code across multiple projects. Role discipline was later strengthened using BMAD as an inspiration reference. A lightweight Agile layer was added to manage backlog and session continuity.

---

## Next development steps

1. **Validate the Architect + Developer + QA pipeline** on a real feature addition in an active project.
2. **Exercise the Security Reviewer role** on a change touching auth, API keys, or user data.
3. **Exercise the DevOps Reviewer role** on a real deployment or CI/CD change.
4. **Test multi-session recovery** using `PROJECT_SUMMARY.md` across at least two sessions.
5. **Add remaining templates:** `DECISION_LOG_TEMPLATE.md`, `PROJECT_BACKLOG_TEMPLATE.md`, `PROJECT_SUMMARY_TEMPLATE.md`, `RETROSPECTIVE_TEMPLATE.md`.
6. **Stress-test `DECISION_LOG.md`** with a real contested architectural decision.
7. **Validate `may-deploy-with-approval`** on a non-production deployment in a controlled project.
8. **Consider versioning the framework** (e.g. `v1.0`, `v1.1`) once the full role pipeline has been validated end-to-end.
