# PROJECT CLAUDE TEMPLATE

Copy this into a project as `CLAUDE.md`. Replace the `<...>` placeholders.
It links the project to the global framework so rules aren't rewritten per project.

---

```markdown
# CLAUDE.md — <PROJECT NAME>

## Global framework (read first)
This project follows the global autonomous development framework:
**~/Projects/_AI_RULES/AUTONOMOUS_DEV_FRAMEWORK.md**

That file is the master index. Load specialist files (agent roles, coding standards,
security, deployment, memory) from there as needed. Its safety rules override anything below.

## How to start a session here
1. Read this file.
2. Read `PROJECT_STATUS.md` for current phase, branch, next task, risks.
3. Read optional memory files when relevant:
   - `PROJECT_SUMMARY.md` — if it exists (fast context recovery).
   - `PROJECT_BACKLOG.md` — if planning work.
   - `DECISION_LOG.md` — if changing architecture, strategy, data models, or deployment.
4. Pick the agent role (see _AI_RULES/AI_AGENT_RULES.md).
5. Follow the WORKFLOW and SAFETY GATES in the framework.

## Project status
- **Stage:** <discovery / building / hardening / maintenance>
- **Priority:** <high / medium / low>
- **Production status:** <not deployed / staging / live>
- **Active branch:** <branch name>
- **Main risk:** <the single biggest risk right now>

## Project profile
- **Purpose:** <one line>
- **Stack:** <languages / frameworks / package manager>
- **Entry points:** <server / CLI / main>

## Commands
- Install: `<cmd>`
- Run/dev: `<cmd>`
- Build: `<cmd>`
- Test: `<cmd>`
- Lint/format/type-check: `<cmd>`

## Conventions
- Branching: `<strategy>`
- Commits: `<style>`
- Tests live in: `<path>`

## Memory files (which may exist in this project)
- `PROJECT_STATUS.md` — current state, branch, last/next task, risks, test/deploy status.
- `PROJECT_SUMMARY.md` — compressed one-page overview for fast context recovery.
- `PROJECT_BACKLOG.md` — epics, stories, priorities, acceptance criteria.
- `DECISION_LOG.md` — major decisions, rationale, alternatives, date.
- `RETROSPECTIVE.md` — what worked/failed and what to change, after milestones.

## Deployment profile
- **Local dev:** `<how to run locally>`
- **Staging:** `<url / how to deploy>`
- **Production:** `<url / how to deploy>`
- **Deployment method:** `<CI/CD pipeline / script / platform>`
- **Rollback method:** `<how to revert a bad release>`

## AI authority level
- **Level:** `<read-only / propose-only / may-edit / may-commit / may-deploy-with-approval>`
- **Notes:** `<scope, exceptions, what still needs human sign-off>`

## Project-specific rules / gotchas
- <anything that overrides or extends the global rules>

## Do-not-touch (this project)
Never modify, delete, or expose without explicit human sign-off:
- production data
- uploads
- databases
- backups
- logs
- secrets
- `.env` files
- user-generated content
- <other project-specific paths, services, or areas>
```
