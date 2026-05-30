# GLOBAL CODING STANDARDS

Operational rules for writing and changing code. Applies to the Developer and Architect roles.
When project conventions conflict with these, follow the project — but state the conflict.

## Golden rule
Match the surrounding code. Read neighboring files first; mirror their naming, structure,
error handling, and comment density. Consistency beats personal preference.

## Before writing code
- Read the files you will change and their immediate callers/callers.
- Confirm the framework, language version, package manager, and test runner in use.
- Reuse existing helpers/utilities before writing new ones. Grep before you create.

## Architecture boundaries
- Respect existing layers and module boundaries (e.g. routes, services, data access, UI).
- Do not move business logic, data access, UI logic, or infrastructure code across layers without a clear reason.
- If the architecture is unclear, switch to the Architect role before editing.

## Change size
- Smallest effective change. One concern per commit/PR.
- Do not reformat unrelated lines. Do not rename things you weren't asked to.
- No drive-by refactors mixed into a feature/fix.

## Safe refactoring
- Refactoring must preserve behaviour. If it changes behaviour, it's a feature, not a refactor.
- Do not mix refactoring with feature work in the same commit unless necessary.
- Run tests before and after refactoring when possible.
- State explicitly what behaviour should remain unchanged.

## API, data, and compatibility
- Do not break public APIs, CLI arguments, config formats, database schemas, file formats, or saved user data without explicit approval.
- Prefer backward-compatible changes. Deprecate before removing.
- If a breaking change is unavoidable, flag it in the PR summary and version it where the project convention requires.

## Configuration discipline
- Do not hardcode secrets, domains, ports, file paths, credentials, or environment-specific values.
- Use existing config/env patterns in the project.
- Add new environment variable names to `.env.example` only — never their values.

## Naming & structure
- Descriptive names; no abbreviations that aren't already used in the repo.
- Keep functions short and single-purpose. Extract only when it improves readability.
- Keep public interfaces stable; deprecate before removing.

## Error handling
- Never swallow errors silently. Log or propagate with context.
- Validate inputs at boundaries (API handlers, CLI args, external data).
- Fail loud in dev, fail safe in prod. No bare `except`/`catch {}` that hides bugs.

## Logging and observability
- Use the project's existing logger — don't introduce a new one without justification.
- Logs must help diagnose problems without exposing secrets, tokens, or personal data.
- Remove noisy debug prints before committing.
- Include enough context in error logs to troubleshoot without a debugger (e.g. operation, ID, error message).

## Performance awareness
- Avoid unnecessary repeated API calls, database queries, file scans, or expensive loops.
- Be careful with large files, large directories, and memory-heavy operations.
- If the performance impact of a change is unknown, flag it in the handoff.

## Generated / AI-written code
- Treat generated code as untrusted until reviewed line by line.
- Ensure it matches the project's style, naming, and structure.
- Ensure behaviour changes introduced by generated code have tests.
- Do not paste large generated blocks without understanding what they do.

## Comments & docs
- Comment the "why", not the "what". Don't narrate obvious code.
- Update README/CHANGELOG/dev docs only when behavior or usage changes.

## Dependencies
- Do not add a dependency for something the stdlib or an existing dep already does.
- New dependency requires justification (Architect role): why, size, maintenance, license.
- Pin/lock per the project's existing convention.

## Tests (see QA role)
- Add or update tests for every behavior change.
- Mirror the existing test layout and naming.
- A change isn't done until relevant tests pass.

## Git hygiene
- Branch names: `feature/...`, `fix/...`, `test/...`, `docs/...`, `refactor/...`.
- Conventional commits: `feat:`, `fix:`, `test:`, `docs:`, `refactor:`, `chore:`.
- Never commit secrets, generated artifacts, or unrelated files.
- Never push to `main`/`master`. See [DEPLOYMENT_RULES.md](./DEPLOYMENT_RULES.md).

## Developer handoff
At the end of a coding task, provide:
- **What changed** — a concise description of the code change.
- **Why** — the reason or ticket/issue it addresses.
- **Files changed** — list of modified files.
- **Tests run** — which suites, pass/fail result.
- **Risks / follow-up** — known gaps, edge cases, tech debt introduced.
- **Recommended next role** — e.g. `QA/Test Architect`, `Security Reviewer`.

## Quick checklist before handing to QA
**Code quality**
- [ ] Builds locally
- [ ] Lint + format + type-check pass
- [ ] No secrets or debug leftovers (`console.log`, `print`, `TODO` blockers)
- [ ] No unrelated file changes
- [ ] Tests added/updated for the change

**Architecture & compatibility**
- [ ] No layer boundaries crossed without justification
- [ ] No public API, schema, or config format broken without approval
- [ ] No hardcoded secrets, domains, or env-specific values

**Safety**
- [ ] No generated code pasted without review
- [ ] Performance risk flagged if unknown
- [ ] Refactor (if any) preserves existing behaviour

**Handoff**
- [ ] Developer handoff note written (what/why/files/tests/risks/next role)
