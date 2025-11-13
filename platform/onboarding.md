# Platform Onboarding Checklist

## Week 1
- [ ] Read the discovery inventory (`docs/platform/discovery.md`) and domain map (`docs/platform/domain_map.md`).
- [ ] Clone all four service repositories and run test suites locally.
- [ ] Configure local `.env` files with shared secrets from 1Password (`Platform/Shared`).
- [ ] Spin up the event gateway (`npm install && npm run dev` in `platform/event-gateway`).
- [ ] Deploy dev infrastructure sandbox via Terraform plan/apply (if sandbox available).

## Week 2
- [ ] Shadow on-call engineer for Kafka / gateway operations.
- [ ] Add a sample schema change + contract test PR using the ADR template.
- [ ] Pair with a squad to wire a new event producer or consumer.
- [ ] Review Grafana + Prometheus dashboards and alert routing.

## Continuous Expectations
- Attend weekly CAB meeting and monthly architecture brown-bag.
- Keep personal runbook notes in `docs/platform/runbooks/<name>.md`.
- Ensure all PRs include contract test evidence when touching schemas.
