# CI/CD Blueprint

## Pipelines
- **Contracts** (`platform/ci/workflows/contracts.yaml`): Validates JSON schemas and executes producer/consumer contract tests. All services import this via `workflow_call`.
- **Tests**: Each repository reuses its native test workflows (RSpec for Rails, Jest/Vitest for Node/React). Ensure coverage reports uploaded to a shared artifact bucket.
- **Deploy**: Terraform plans run in GitHub Actions using environment protection rules. Production deploys require manual approval from platform + domain owner.

## Branch Policies
- Main branch protected with required checks (`contracts`, `tests`, `lint`).
- Feature branches must reference Jira ticket IDs and include ADR links for schema/infra changes.

## Release Flow
1. Merge to `main` triggers staging deployment.
2. Automated smoke tests hit event gateway health, Kafka topics, and sample producer endpoints.
3. Production deploy triggered via workflow dispatch after CAB sign-off.

## Toolchain
- GitHub Actions + reusable workflows (`platform/ci`). Shared templates:
  - `rails-service-ci.yaml` for Rails services (pickup-game-manager, saturday_league_football)
  - `node-monorepo-ci.yaml` for the Turborepo (`sarradabet`)
  - `react-vite-ci.yaml` for SPA frontends (`saturday_league_football_frontend`)
  - `contracts.yaml` for producer/consumer contract tests
- Dependabot updates for Node/Ruby dependencies weekly.
- Code owners configured per directory to enforce domain ownership.
