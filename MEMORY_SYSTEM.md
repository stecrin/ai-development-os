# MEMORY SYSTEM

How to carry context across sessions so the agent never re-learns a project from scratch.

## Memory authority order
When sources conflict, the higher-priority source wins:

1. **User instruction in the current session** — always wins.
2. **Project `CLAUDE.md`** — project rules and pointers.
3. **`PROJECT_STATUS.md`** — current operational state.
4. **`DECISION_LOG.md`** — recorded decisions and rationale.
5. **`README` / docs** — general project documentation.
6. **Global `_AI_RULES`** — defaults when nothing more specific applies.

Secrets are **never** memory — never read, store, or carry credentials/`.env` values (see pollution rule below).

## Where memory lives
- **Per-project (primary):** `PROJECT_STATUS.md` in the project root — see
  [PROJECT_STATUS_TEMPLATE.md](./PROJECT_STATUS_TEMPLATE.md). This is the source of truth for
  current state.
- **Per-project profile:** the filled-in [PROJECT_ONBOARDING_TEMPLATE.md](./PROJECT_ONBOARDING_TEMPLATE.md)
  output (stack, commands, conventions) stored in the project (e.g. in `CLAUDE.md` or `docs/`).
- **Global:** these `_AI_RULES` files — rules that apply to every project.

## Recommended per-project memory files
Create these in the project root as needed (don't create them all upfront — add when relevant):

- **`PROJECT_STATUS.md`** — current state, branch, last task, next task, risks, test/deploy status.
- **`DECISION_LOG.md`** — major decisions, rationale, alternatives considered, date.
- **`PROJECT_BACKLOG.md`** — epics, stories, priorities, acceptance criteria.
- **`PROJECT_SUMMARY.md`** — compressed one-page overview for fast context recovery.
- **`RETROSPECTIVE.md`** — what worked, what failed, what should change, after major milestones.

## Lightweight Agile memory
Track work with the minimum useful Agile vocabulary:

- **Epics** = large outcomes.
- **Stories** = small executable tasks.
- **Acceptance criteria** = how we know a story is done.
- **Backlog** = prioritised list of work.
- **Retrospective** = learning captured after major work.

Use **lightweight Agile only**. Avoid corporate Scrum bureaucracy (ceremonies, points, sprints)
unless the project has multiple humans who need it.

## Session start
1. Read the project's `CLAUDE.md` → it points here.
2. Read `PROJECT_STATUS.md` to recover: phase, active branch, last task, next task, risks, test/deploy status, open decisions.
3. Read `PROJECT_SUMMARY.md` **if it exists** (read it first after `CLAUDE.md` on large projects — see compression rule).
4. Read `PROJECT_BACKLOG.md` **if planning work**.
5. Read `DECISION_LOG.md` **if changing architecture or strategy**.
6. If `PROJECT_STATUS.md` is missing → run onboarding first.

## Session end
- **`PROJECT_STATUS.md`** — always: last task, next task, new/resolved risks, test + deploy status, open decisions.
- **`DECISION_LOG.md`** — if a decision was made.
- **`PROJECT_BACKLOG.md`** — if priorities changed.
- **`PROJECT_SUMMARY.md`** — after major milestones.
- **`RETROSPECTIVE.md`** — after major releases or failures.

## Memory compression rule
For large projects, maintain `PROJECT_SUMMARY.md` as a single compressed page and treat it as the
**first file read after `CLAUDE.md`**. It exists so context can be recovered fast without reading
everything. Keep it short; refresh it after major milestones.

## What to persist
- Decisions and their rationale (so they aren't re-litigated).
- Project-specific conventions discovered (build/test/deploy commands, gotchas).
- Known risks and tech debt.
- Where things live (entry points, config, deploy scripts).

## Memory pollution rule (what NOT to persist)
Do not save:
- secrets, tokens, credentials, `.env` values — never.
- obvious facts visible from the code.
- temporary debugging guesses.
- emotional or conversational comments.
- duplicate notes.
- stale plans.
- transient chat details irrelevant to future work.
- anything already captured by the repo itself (code structure, git history, README).

## Hygiene
- One fact per place; update existing notes instead of duplicating.
- Keep entries short and operational. Delete notes that become wrong.
- Convert relative dates to absolute (e.g. "next week" → a real date).
