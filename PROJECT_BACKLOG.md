# PROJECT_BACKLOG — AI Development OS

---

## How to use this file

Each item follows the framework's Agile vocabulary:
- **Story:** small, executable task with a clear done condition
- **Acceptance criteria:** how we know it's done
- **Priority:** High / Medium / Low
- **Status:** Planned / In Progress / Done / Blocked

---

## Backlog

---

### Add Context Management Rules

**Priority:** High
**Status:** Completed

**Completion note:**
The Minimal Filesystem & Context Discipline Rule was implemented across:
- `CONTEXT_MANAGEMENT_RULES.md` (created)
- `AI_AGENT_RULES.md` (rule added to Role rules section)
- `AUTONOMOUS_DEV_FRAMEWORK.md` (specialist file entry added; three lines added to ABSOLUTE DO NOT)

The rule was created after a live validation issue where Claude attempted to use 1M context and performed unnecessary filesystem actions during the Home Lab verification sprint.

**Problem:**
Large projects can exceed context limits or become confused when too much raw material is pasted repeatedly. Long AI Development OS workflows break down when prompts are overloaded with raw logs, repeated screenshots, and outdated assumptions accumulated across sessions.

**Story:**
Create `CONTEXT_MANAGEMENT_RULES.md` — a specialist rule file that defines how AI-assisted workflows should manage context size and clarity throughout a session.

**Required rules to include:**
- Split large workflows into batches
- Use Batch 0 clarification before execution
- Avoid repeating huge raw outputs in subsequent prompts
- Create compact verification summaries after each batch
- Separate raw command output from verified conclusions
- Keep final source-of-truth files short
- Mark assumptions, verified facts, and unknowns clearly
- Force Claude to read only task-relevant files
- Create handoff summaries before moving to a new stage

**Acceptance criteria:**
- `CONTEXT_MANAGEMENT_RULES.md` exists with all required rules
- Rules are operational and checklist-driven (not vague)
- File is referenced in `AUTONOMOUS_DEV_FRAMEWORK.md` (master index)
- File is listed in `README.md` under Core components
- File is short enough to load per-session without inflating context itself
