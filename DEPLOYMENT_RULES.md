# DEPLOYMENT RULES

Operational rules for branching, committing, CI/CD, and releasing. Owned by the
DevOps/Deployment Reviewer role. Reinforces [AUTONOMOUS_DEV_FRAMEWORK.md](./AUTONOMOUS_DEV_FRAMEWORK.md).

## Hard rules
- NEVER push directly to `main`/`master`.
- NEVER deploy to production automatically or without explicit human approval.
- NEVER run destructive migrations or commands against production data.
- ALWAYS preserve the existing deployment structure (scripts, CI config, Dockerfiles, infra).

## Deployment authority
- **Default:** Claude may prepare and describe deployment steps but must not execute a production deployment without explicit human approval in the current session.
- **If `CLAUDE.md` or `PROJECT_STATUS.md` sets `may-deploy-with-approval`:** still stop and obtain final human confirmation before any production deployment. Approval in a past session does not carry forward.

## Branching
- Branch before editing: `feature/...`, `fix/...`, `test/...`, `docs/...`, `refactor/...`.
- One logical change per branch. Keep branches short-lived.
- Rebase/merge per the project's existing convention — don't change the strategy.

## Commit gate (all must be true)
- [ ] Change is complete
- [ ] Tests pass (see QA role)
- [ ] No secrets exposed (see [SECURITY_RULES.md](./SECURITY_RULES.md))
- [ ] No unrelated files changed
- [ ] Conventional commit message (`feat:`/`fix:`/`test:`/`docs:`/`refactor:`/`chore:`)
- [ ] No `.env` edited without explicit human instruction
- [ ] Migration (if any) is reversible or has a documented recovery plan

## Environment variable discipline
- Check that required variable names exist in the target environment — never print or copy values.
- Never edit `.env` directly unless explicitly instructed by the human in the current session.
- Add new variable names to `.env.example` only (names, never values).

## Database and migration safety
- Never run destructive migrations (drop, truncate, irreversible alter) without human approval.
- Prefer additive, reversible migrations. Avoid multi-step irreversible changes in a single migration.
- Back up the database before any risky migration.
- Test the full migration path (apply + rollback) in a non-production environment first.
- Every migration must include a rollback or documented recovery plan.

## CI/CD
- Run the project's pipeline checks locally first when possible (build, lint, type, test).
- Do not disable, skip, or weaken existing CI checks to make a build pass.
- If CI is red, fix the cause — don't merge around it.

## Service and infrastructure awareness
- **Docker containers:** do not change image tags, restart policies, or volume mounts in production without approval.
- **Systemd services:** confirm the service restarts cleanly after changes; check `systemctl status` post-deploy.
- **Reverse proxy:** do not modify nginx/caddy/haproxy config in production without a tested backup config and rollback plan.
- **TLS certificates:** confirm certs are valid and auto-renewal is in place before any domain/proxy change.
- **Ports/firewall:** do not open new ports or change firewall rules without human approval.
- **Logs:** confirm log rotation is in place; check logs immediately after deployment.
- **Backups:** verify a recent backup exists and is restorable before any risky operation.

## Release / promote
- Promote through existing environments in order (e.g. dev → staging → prod).
- Confirm migrations are reversible and run in the right order.
- Confirm env vars/config exist in the target environment (by name — never copy secret values).

## Pull Request summary (required)
- **What changed**
- **Why**
- **Files modified**
- **Tests run** (and result)
- **Risks**
- **Rollback instructions**

## Pre-deployment checklist
Before any deployment, verify all of the following:
- [ ] Correct branch (not `main`/`master` unless merging via PR)
- [ ] Working tree clean or all changes documented
- [ ] Tests, build, and lint passed
- [ ] No secrets exposed (see [SECURITY_RULES.md](./SECURITY_RULES.md))
- [ ] Required env var names confirmed in target environment (values not inspected)
- [ ] Backup status checked
- [ ] Rollback path written and confirmed
- [ ] Production risk level understood and accepted
- [ ] Human approval obtained for any production deployment

## Post-deployment verification
After deployment, confirm:
- [ ] App/service is reachable at the expected URL or port
- [ ] Health endpoint returns OK (if available)
- [ ] Logs show no new critical errors
- [ ] Key user flow works end-to-end
- [ ] Background services and workers are running
- [ ] Deployed version/commit matches what was intended

## Failed deployment response
If a deployment fails:
1. Stop — do not retry blindly.
2. Capture the full error output.
3. Determine whether a rollback is needed immediately.
4. Do not make additional unrelated changes while the system is in a degraded state.
5. Report clearly: what failed, current system state, and the next safest action.

## Rollback
- Every deployable change needs a stated rollback path before merge.
- Prefer reversible changes (feature flags, additive migrations) over irreversible ones.

## Verdict
End with: `DEPLOY: ready` / `blocked` / `needs-human-review` + reason.
