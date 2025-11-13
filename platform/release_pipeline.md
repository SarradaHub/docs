# Release & Promotion Pipeline

## Branch Strategy
- `feature/*`: short-lived branches with PRs into `develop` (optional) or directly into `main` with review.
- `develop`: optional integration branch for shared testing (maps to staging deployment).
- `main`: production-ready; protected with required status checks and approvals.

## Pipeline Stages
1. **CI (All PRs)**
   - Lint, unit tests, contract tests via `platform-ci.yml` workflows.
   - Preview deployments (Vercel preview, Render PR deploy) triggered automatically.
2. **Staging Deploy**
   - Triggered on merge to `develop` (or `main` with flag).
   - Terraform applies to `envs/staging` (requires manual approval via GitHub Environment `staging`).
   - Smoke tests hit event gateway `/health`, API endpoints, frontend ticker SSE.
3. **Production Deploy**
   - Workflow dispatch `Deploy Production` requiring CAB approval.
   - Applies Terraform `envs/prod`, runs Kamal/Render/Vercel promotions.
   - Post-deploy verification script ensures event roundtrip (match scheduled → bet market → frontend update).

## Approvals & Guards
- GitHub Environments `staging` and `prod` require reviewers from Platform + Domain owner groups.
- Deployment freeze calendar integrated with GitHub (`protected` schedules) for major events.
- Automatic rollback actions prepared (`terraform apply -refresh-only`, Kamal rollback, Render deploy from previous commit).

## Secrets Management
- Secrets stored in GitHub Environment-level vault.
- Sync jobs propagate secrets to Vercel/Render/Supabase after approval using provider APIs.
