# CONTEXT MANAGEMENT RULES

Load this file when: a task involves multiple steps, large outputs, repeated reads, or long sessions where context overload is a risk.

---

## Minimal Filesystem & Context Discipline Rule

Agents must not perform extra filesystem actions unless required for the task.

**Accepted actions:**
- Read only explicitly named source files.
- Create or edit only explicitly named target files.
- Create a parent folder only when necessary for the requested output.
- Run narrow verification commands only when requested.

**Not accepted:**
- Listing directories after completing a task.
- Scanning the full repository without permission.
- Reading unrelated files.
- Running broad search commands.
- Continuing after the requested output is complete.
- Using 1M context for routine documentation, summaries, checklists, diagrams, or small edits.

---

## Stop and report rule

After completing a task, the agent must stop and report only:
- File created or modified
- Short summary of what changed
- Issues found (if any)
- Next recommended step

Do not continue to scan, list, verify, or summarise beyond what was requested.

---

## Batch discipline

- Split large workflows into batches with defined scope.
- Use a Batch 0 clarification step before execution to confirm scope, files, and expected outputs.
- Do not paste the same large raw output more than once across batches.
- After each batch, produce a compact verification summary — not a repeat of raw output.

---

## Context hygiene

- Separate raw command output from verified conclusions.
- Keep final source-of-truth files short. If a file grows large, compress it into a summary file.
- Mark assumptions, verified facts, and unknowns explicitly.
- Create a handoff summary before moving to a new stage or session.
- Force reads to only task-relevant files. Do not load memory files that are not needed for the current step.
