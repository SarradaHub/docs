# CI/CD Blueprint

## Pipelines
- **Contracts** (`.github/workflows/contracts.yaml`): Validates JSON schemas and executes producer/consumer contract tests. All services import this via `workflow_call`.
- **Rails Service CI** (`.github/workflows/rails-service-ci.yaml`): Comprehensive CI workflow for Rails services including linting (RuboCop), security (Brakeman), quality metrics (RubyCritic), and testing (RSpec with PostgreSQL).
- **Node Monorepo CI** (`.github/workflows/node-monorepo-ci.yaml`): CI workflow for Node.js monorepos with linting, type checking, building, and testing. Supports Prisma ORM and PostgreSQL with coverage artifacts.
- **React Vite CI** (`.github/workflows/react-vite-ci.yaml`): CI workflow for React/Vite frontend projects with linting (ESLint), formatting (Prettier), type checking (TypeScript), testing (Vitest), and building.
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
- GitHub Actions + reusable workflows (`.github/workflows/` in the platform repository).
- Dependabot updates for Node/Ruby dependencies weekly.
- Code owners configured per directory to enforce domain ownership.
