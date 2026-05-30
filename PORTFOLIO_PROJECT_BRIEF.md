# PORTFOLIO PROJECT BRIEF

## Project working title
**Personal AI Development Operating System**
*(working title — may be refined before publishing)*

---

## One-sentence description
A structured, safety-first framework for working with Claude Code across real software projects — combining global AI rules, BMAD-inspired agent roles, lightweight Agile workflow, project memory, and human approval gates.

---

## The starting problem
Working with AI coding assistants across multiple projects is powerful but fragile. Without structure, the same onboarding has to happen in every session, context is lost between conversations, safety rules aren't enforced consistently, and there's no clear sense of what the AI is and isn't allowed to do.

The specific problems I wanted to solve:
- Every session started cold, with no memory of the project.
- There were no role boundaries — the AI would try to do everything at once.
- Safety gates (secrets, production data, deployments) weren't systematic.
- Git discipline depended on per-prompt reminders rather than embedded rules.
- There was no way to hand a project to the AI and say "here's the context, pick up where we left off."

---

## How the idea began
I started by creating a global rules folder at `~/Projects/_AI_RULES`. The goal was simple: one place where I could define how AI should behave across every project I work on, without rewriting instructions from scratch each time.

The initial structure included:
- A master framework file (`AUTONOMOUS_DEV_FRAMEWORK.md`) with core safety rules and a development workflow.
- Planned files for security rules, deployment rules, coding standards, and memory — some with initial content, some empty placeholders ready to be filled.
- The idea that each project would have a `CLAUDE.md` pointing back to the global framework, so context could be loaded consistently.

At this stage, it was personal, practical, and opinionated. It reflected how I wanted to work — step by step, safely, with Git discipline, and with clear human approval before anything irreversible.

---

## Discovering BMAD through Bogdan
A friend of mine, Bogdan, who works in IT, mentioned BMAD during a conversation. I looked into it and recognised something familiar: it had a similar instinct toward structured agent roles, controlled handoffs, and separated responsibilities.

What stood out was not that BMAD invented these ideas, but that it had formalised them in a way that matched what I was already trying to build. It was a useful reference point — a signal that the direction I had taken independently was sound.

---

## How BMAD influenced the framework
I did not replace my system with BMAD, and I did not copy it. I used it as an inspiration layer to sharpen what I already had — specifically:

- **Agent roles:** I added a formal set of named roles — Strategic Advisor, Analyst, Product Manager, Architect, Developer, QA/Test Architect, Security Reviewer, DevOps Reviewer — each with a clear mission, activation condition, and handoff.
- **Structured handoffs:** Each role produces a defined output that the next role consumes.
- **Task separation:** The framework now enforces that roles don't blur. An Analyst doesn't write code. A Developer doesn't plan architecture.
- **Controlled development flow:** The pipeline from Strategic Advisor down to DevOps Reviewer mirrors a real review process, adapted for solo AI-assisted work.

The result is a hybrid: my original global rules structure, strengthened by BMAD-style role discipline.

---

## Why Agile was added
Once the role structure was in place, the missing piece was project tracking. Without it, the AI had no structured way to answer: *what are we building, what's next, and how do we know when it's done?*

I added a lightweight Agile layer — deliberately minimal, avoiding ceremony:
- **Epics and stories** to define what's being built and why.
- **Acceptance criteria** so "done" has a clear definition.
- **Backlog** as a prioritised work list.
- **Decision log** so architectural choices aren't re-litigated in future sessions.
- **Retrospective** for learning after major milestones.
- **"Done when" conditions** attached to every next task.
- **Human approval checkpoints** before commits, deployments, and any safety-gate area.

The rule is: lightweight Agile only. No sprints, no standups, no story points unless the project genuinely has multiple humans who need them.

---

## What the system includes

### Global framework files (`~/Projects/_AI_RULES`)
| File | Purpose |
|------|---------|
| `AUTONOMOUS_DEV_FRAMEWORK.md` | Master index. Safety rules, workflow, orchestration, AI authority rule, output format. |
| `AI_AGENT_RULES.md` | 8 BMAD-inspired roles with missions, activations, outputs, and handoffs. |
| `GLOBAL_CODING_STANDARDS.md` | Code quality rules: architecture boundaries, safe refactoring, compatibility, config discipline, logging, performance, generated code, developer handoff. |
| `SECURITY_RULES.md` | Secrets, production data, input handling, external APIs/webhooks, file uploads, infrastructure, AI-specific security, severity levels. |
| `DEPLOYMENT_RULES.md` | Deployment authority, branching, commit gates, migration safety, CI/CD, pre/post-deploy checklists, failed deployment response, rollback. |
| `MEMORY_SYSTEM.md` | Memory authority order, per-project memory files, lightweight Agile vocabulary, session start/end routines, compression rule, pollution rule. |

### Templates (copy into each project)
| Template | Purpose |
|----------|---------|
| `PROJECT_CLAUDE_TEMPLATE.md` | Each project's `CLAUDE.md`. Points to global framework. Holds stack, commands, memory files, deployment profile, AI authority level, do-not-touch list. |
| `PROJECT_ONBOARDING_TEMPLATE.md` | One-time Analyst inspection when entering a new project. Covers stack, commands, conventions, risk scan, memory file check, Agile context. |
| `PROJECT_STATUS_TEMPLATE.md` | Living status file. Phase, branch, priority, risk level, Agile context, next task, files changed, tests run, AI authority level, human review flag. |

### Per-project memory files (created during onboarding)
- `PROJECT_STATUS.md` — current operational state.
- `PROJECT_SUMMARY.md` — compressed one-page overview for fast recovery.
- `PROJECT_BACKLOG.md` — epics, stories, priorities, acceptance criteria.
- `DECISION_LOG.md` — major decisions, rationale, alternatives, date.
- `RETROSPECTIVE.md` — learning after major milestones.

---

## Validation on LegionTrap
The first real validation was run on **LegionTrap TI**, an active project at `~/Projects/gitrepo/legiontrap-ti`.

What was tested and confirmed working:
- Project onboarding using the framework's templates.
- Memory file creation: `CLAUDE.md`, `PROJECT_STATUS.md`, `PROJECT_SUMMARY.md`, `PROJECT_BACKLOG.md`, `DECISION_LOG.md`.
- Controlled Analyst task: inspecting `.bak` files, temp files, `node_modules`, `.gitignore`.
- Controlled Developer task: removing tracked `.bak` files from Git, fixing `.gitignore` conflict residue.
- Second narrow Analyst task: inspecting `tmp_events_test.jsonl`, followed by a separate approved cleanup step.
- Full PR workflow: `docs/` and `chore/` branches → commits → GitHub pull requests → merges into main → branch deletion.
- Human approval at every stage. No autonomous deploys or unreviewed commits.

The validation confirmed that the framework is operational for low-risk onboarding and cleanup tasks on a real project. It is not yet validated for feature development, migrations, or production deployments — those are the next testing targets.

---

## What makes it portfolio-worthy
- It is a real, working system used on real projects — not a demo or tutorial exercise.
- It reflects original thinking about a problem that matters: how to work safely and systematically with AI coding assistants at the project level.
- It combines several disciplines — AI tooling, software engineering process, security, DevOps, Agile — into a single coherent personal operating system.
- It was built iteratively, with each file tested and refined rather than written once and left.
- The validation story is honest: it documents what works, what was tested, and what still needs validation — no inflation.

---

## Skills demonstrated
- **AI tooling and prompt engineering** — designing structured instructions that reliably guide LLM behaviour across varied tasks.
- **Software engineering process** — branch discipline, commit conventions, PR workflow, code review gates.
- **Security thinking** — systematic handling of secrets, production data, input validation, deployment authority.
- **DevOps awareness** — pre/post-deploy checklists, rollback planning, migration safety, environment discipline.
- **Technical writing** — precise, operational documentation that is actually used, not written and forgotten.
- **Systems thinking** — designing a framework that composes well: global rules → project overrides → session memory → role pipeline → human gates.
- **Iterative development** — the system was built in stages, with each improvement grounded in real use.

---

## Suggested portfolio website structure

```
Project: Personal AI Development Operating System

[Hero]
A structured framework for working with Claude Code across software projects.
Designed from my own workflow. Tested on a real project. Safety-first.

[The Problem]
Working with AI without structure loses context, blurs responsibilities, and skips safety.

[The Solution]
A global rule system + agent roles + project memory + human approval gates.

[How It Works]
1. Global framework defines rules and roles.
2. Each project connects via CLAUDE.md.
3. Agent roles separate planning from building from reviewing.
4. Memory files carry context between sessions.
5. Human approval required before anything irreversible.

[What's Inside]
→ 6 specialist rule files
→ 3 project templates
→ 5 per-project memory files
→ 8 BMAD-inspired agent roles
→ Lightweight Agile layer

[Validated On]
LegionTrap TI — real project, real PR workflow, real branches, real merges.

[GitHub / Source]
[link]
```

---

## Short website summary
*(copy-paste ready)*

> I built a personal AI development operating system for working with Claude Code across my projects. It combines a global rule framework, structured agent roles inspired by BMAD, lightweight Agile project tracking, and per-project memory files. Every session starts with context. Every significant change can go through a defined role pipeline. Every deployment requires human sign-off. It was validated on a real project with full GitHub PR workflow. This is how I work with AI — deliberately, safely, and repeatably.

---

## Important wording rules
*(apply these whenever describing this project publicly)*

- **Do not say:** "fully autonomous AI system."
  **Say instead:** "structured framework for AI-assisted development."

- **Do not say:** "I built this based on BMAD."
  **Say instead:** "BMAD was discovered later and used as an inspiration layer to strengthen the role structure."

- **Do not say:** "production-ready autonomous agent."
  **Say instead:** "validated for controlled, human-approved tasks on real projects."

- **Do not say:** "I copied the agent roles from BMAD."
  **Say instead:** "agent role discipline was informed by BMAD principles and adapted to my own framework."

- **Do not overstate the validation scope.** LegionTrap tested onboarding, memory, Analyst, Developer, and PR workflow. Feature development, Security Reviewer, DevOps Reviewer, and deployment flows are not yet validated.
