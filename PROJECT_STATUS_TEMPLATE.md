# PROJECT STATUS TEMPLATE

Copy this into a project as `PROJECT_STATUS.md`. This is the living memory of the project.
Read it at session start; update it at session end (see [MEMORY_SYSTEM.md](./MEMORY_SYSTEM.md)).

---

```markdown
# PROJECT STATUS — <PROJECT NAME>

_Last updated: <YYYY-MM-DD> by <agent/human>_

## Current phase
<e.g. onboarding / building feature X / hardening / maintenance>

## Project priority
- **Level:** <high / medium / low>
- **Reason:** <why this priority>

## Production risk level
- **Level:** <low / medium / high>
- **Why:** <what makes it risky right now>

## Active branch
<branch name, or "none — on default, need to branch before editing">

## AI authority level
<read-only / propose-only / may-edit / may-commit / may-deploy-with-approval>

## Current Agile context
- **Current epic:** <large outcome being worked toward>
- **Current story:** <the specific task in progress>
- **Acceptance criteria:** <how we know this story is done>
- **Backlog:** see `PROJECT_BACKLOG.md` if used

## Last completed task
<what was finished, with date>

## Next task
- **Action:** <the single next concrete action>
- **Why it matters:** <impact or unblocking reason>
- **Done when:** <the specific condition that marks it complete>

## Files changed this session
- `<path>` — <reason>

## Commands / tests last run
- **Command:** `<cmd>`
  **Result:** <pass / fail / partial>
  **Date:** <YYYY-MM-DD>
  **Notes:** <anything unexpected>

## Known risks
- <risk + severity + mitigation/owner>

## Test status
<pass / fail / not run — which suites, last run date>

## Deployment status
<not deployed / deployed to staging / prod version X — last deploy date>

## Human review required
- **Required:** <yes / no>
- **Reason:** <what needs sign-off and why>

## Open decisions
- <decision needed> — options: <a / b> — owner: <who> — blocking? <yes/no>

## Notes
- <short operational notes, gotchas, links>
```
