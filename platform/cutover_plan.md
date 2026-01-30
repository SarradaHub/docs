# Cutover Plan

## Prerequisites
- All CI workflows green on `main` across repositories.
- Terraform plans approved for `staging` and `prod` environments.
- Event gateway image pushed and smoke-tested in staging.
- Secrets synchronized to Vercel, Render, Kamal, and Supabase.

## Timeline
1. **T-7 days**: Freeze schema changes; run load tests on staging gateway.
2. **T-1 day**: Backup Supabase + Postgres databases; confirm MSK cluster healthy; notify stakeholders of deployment window.
3. **Cutover Window**:
   - Scale down legacy integrations (if any) after ensuring backlog is empty.
   - Apply Terraform to production (`infra/terraform/envs/prod`).
   - Deploy event gateway and confirm `/health`.
   - Deploy producer services (Pickup Game Manager, Saturday League API, SarradaBet API).
   - Promote Vercel frontends to production.
   - Run smoke tests (see `docs/platform/smoke_tests.md`).
4. **Post-cutover (T+1 hr)**: Review metrics dashboards, verify alert silence, communicate success.

## Rollback Strategy
- Gateway: redeploy previous container image tag; revert Terraform changes via `terraform rollback` (apply previous state or `terraform apply -refresh-only`).
- Services: use Kamal/Render rollback commands to deploy previous version.
- Frontends: Vercel `--scope` rollback to previous deployment.
- Data: restore Supabase snapshot if migrations failed.

## Communication Plan
- Primary channel: Slack `#platform-launch`.
- Incident bridge contact: Platform on-call engineer + domain leads.
- Status updates every 15 minutes during cutover; final summary posted post-launch.

## Acceptance Criteria
- New matches trigger betting markets within <30s (verified via smoke tests).
- Payments appear in unified ledger topics.
- Frontend ticker displays live updates without errors.
- No critical alerts active post launch.
