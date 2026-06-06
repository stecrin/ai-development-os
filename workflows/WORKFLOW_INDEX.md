# AI Development OS — Workflow Layer

## What this layer is

Workflows turn common development situations into repeatable, step-by-step operating procedures.

The global framework defines the rules (what Claude may or may not do).
The agent roles define the function (Analyst inspects, Developer implements, QA verifies).
The workflow layer defines the sequence (what to do, in what order, with what gates, for a specific situation).

A workflow is not a script Claude runs automatically. It is a structured procedure that a human invokes, reads, and approves at each gate.

---

## How workflows relate to roles

Each workflow specifies a recommended role. That role determines what Claude is permitted to do within the workflow — no more, no less.

The role definition in [AI_AGENT_RULES.md](../AI_AGENT_RULES.md) is always authoritative. If a workflow appears to conflict with a role boundary, the role boundary wins.

---

## How to choose a workflow

Match your situation to the description below. If no workflow fits, use the Default Analyst Prompt from [AIOS_QUICK_START.md](../AIOS_QUICK_START.md) instead.

| Situation | Workflow |
|-----------|----------|
| I returned to a project and don't fully remember its state | [project-inspection.md](./project-inspection.md) |
| I am about to commit and want a final safety check | `pre-commit-review.md` *(planned)* |
| I need to update or create documentation | `documentation-update.md` *(planned)* |

---

## Available workflows

### project-inspection.md

**Status:** Available

**Use when:** You are returning to a project after a break, entering a project for the first time this session, or your mental model of the project's current state is uncertain.

**What it does:** Guides Claude through a targeted, read-only inspection of the project's most important state files. Claude reports verified facts, flags assumptions, and recommends the first safe next action.

See [project-inspection.md](./project-inspection.md) for the full procedure.

---

## Planned workflows

### pre-commit-review.md

**Status:** Planned

**Planned use:** Before any commit. Runs git diff, checks for secrets, verifies the branch name, confirms the commit message convention, checks no unrelated files are staged, and reports a commit readiness verdict.

### documentation-update.md

**Status:** Planned

**Planned use:** When a README, guide, reference file, or in-project documentation needs to be created or updated. Guides Claude through reading existing docs, drafting, and requesting approval before writing.

---

## Human approval rule

Workflows operate under the principle of controlled autonomy.

Claude may: inspect files, analyse findings, report facts, flag risks, separate verified facts from assumptions, and recommend the next safe action.

Human approval is required before any of the following:

- Editing or creating files
- Creating or switching branches
- Committing changes
- Pushing to any remote
- Opening or merging pull requests
- Deleting anything
- Deploying to any environment
- Touching authentication, authorization, or permissions
- Touching secrets, tokens, API keys, or environment variables
- Touching databases, migrations, or production data
- Touching external APIs, webhooks, or infrastructure

If a workflow step reaches any of the above, Claude must stop, state what it found, and ask for explicit permission before continuing.

Approval given in a past session does not carry forward to the current session.
