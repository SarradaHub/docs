# Governance & Operating Model

## Roles & Responsibilities
- **Platform Team**: Owns shared infrastructure (Kafka, schema registry, observability), event gateway, and contract governance.
- **Domain Squads**: Continue shipping features inside their bounded context; responsible for adhering to published schemas and running contract tests before merges.
- **Change Advisory Board (CAB)**: Weekly 30-minute sync led by platform architect; reviews upcoming schema changes, infra modifications, and cross-cutting dependencies.
- **Incident Commander Rotation**: Shared rota covering event backbone incidents with playbooks documented in `docs/platform/runbooks` (to be expanded).

## Change Management
1. Raise an Architecture Decision Record (ADR) using the template under `docs/platform/templates/adr.md` (create PR with context + decision).
2. For schema changes:
   - Update JSON Schema + AsyncAPI artifacts in `platform/contracts`.
   - Run contract tests locally (`npm run contract:test` in each service).
   - Obtain CAB approval (label PR with `schema-approved`).
3. For infra changes:
   - Modify Terraform under `infra/terraform`.
   - Run `terraform plan` per environment and attach plan output to PR.
   - Ensure cost impact reviewed by platform engineering + finance.

## CI/CD Standards
- All repositories adopt the shared workflow templates in `platform/ci` (import via GitHub Actions `workflow_call`).
- Required checks before merge:
  - `contracts` – validates JSON schemas + runs producer/consumer contract tests.
  - `tests` – executes unit/integration suites per service.
  - `lint` – ESLint/Style checks for TypeScript + RuboCop/Brakeman for Rails.
- Release trains:
  - **Weekly**: Staging deployment freeze every Wednesday 18:00 UTC, production cut on Thursday after smoke tests.
  - **Hotfix**: bypass only with incident commander + product owner approvals.

## Observability & Incident Response
- Centralized dashboards in Managed Grafana (per environment) with panels for Kafka lag, gateway throughput, schema validation errors.
- Alerts configured via CloudWatch Alarms -> Slack channel `#platform-alerts`.
- MTTR target: < 45 minutes; incident reviews must be documented in `docs/platform/postmortems/` within 3 business days.

## Data Governance
- Schema registry enforces backward compatibility; breaking changes require deprecated version removal plan.
- PII is limited to identity service; downstream events include hashed identifiers when possible.
- Access to Kafka topics controlled via IAM roles; least-privilege policies maintained in Terraform.

## Onboarding & Enablement
- New engineers complete the platform onboarding checklist (`docs/platform/onboarding.md`).
- Contract test harness examples live under `platform/ci/examples` for quick adoption.
- Monthly brown-bag covering event design best practices and infra updates.
