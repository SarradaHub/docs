<!-- Drafted by GPT-5 Codex on behalf of the SarradaHub team -->
# SarradaHub Target Architecture

## Current State Snapshot

### Services and Contexts
| Service | Context | Core Responsibilities | Primary Tech | Data Stores |
| --- | --- | --- | --- | --- |
| `platform/event-gateway` | Platform | Validates canonical events, routes to Kafka | Node.js, Kafka | AWS MSK, Glue Schema Registry |
| `saturday_league_football` | Scheduling | Manage championships, matches, rosters, stats | Rails 8 | PostgreSQL (`saturday_league_football_*`) |
| `sarradabet/apps/api` + `apps/web` | Betting | Bet markets, odds lifecycle, fan UI | TypeScript (Express + React) | PostgreSQL (`sarradabet`), Redis (planned) |
| `pickup-game-manager` | Operations Finance | Attendance, payments, income/expense ledger | Rails 8 | PostgreSQL (`pickup_game_manager_*`) |
| `saturday_league_football_frontend` | Experience Web | Public dashboards, match management UI | React 19 | Vercel (static assets) |

### Platform Capabilities
- Canonical event schemas in `platform/contracts` governed through AsyncAPI and JSON Schema registry.
- Shared CI workflows (`platform/ci`) enforcing contract validation and dependency hygiene.
- Terraform stack provisioning AWS network, MSK/Kafka, schema registry, observability foundation, and SaaS integrations such as Render, Vercel, and Supabase.
- Documentation for governance, observability, runbooks, and ops processes under `docs/platform`.

### Infrastructure Baseline
- AWS VPC across three availability zones with managed NAT gateways.
- Amazon MSK for event streaming, AWS Glue Schema Registry for contract management.
- Amazon Managed Prometheus & Grafana for metrics, CloudWatch log aggregation.
- Vercel for frontend deployments, Render for API workloads, Supabase integrations where managed Postgres is acceptable.

## Guiding Principles
- **Scale through decoupled services.** Ensure each bounded context owns its domain logic, data store, and contracts while communicating via an event backbone. Reference the separation and anti-coupling patterns cataloged in the Awesome Scalability principles and case studies [https://github.com/binhnguyennus/awesome-scalability?tab=readme-ov-file#principle].
- **Resilience first.** Target graceful degradation, idempotent processors, retries with backoff, and bulkhead isolation around critical workflows (bet settlement, payments). Leverage chaos testing and failure budgeting aligned with proven internet-scale practices [https://github.com/binhnguyennus/awesome-scalability?tab=readme-ov-file#principle].
- **Observability everywhere.** Adopt structured logging, distributed tracing, and kardinal SLOs so every service advertises golden signals (latency, traffic, errors, saturation). Extend the existing managed Prometheus/Grafana stack with OpenTelemetry collectors.
- **Data governance and contract discipline.** Keep schemas versioned, backwards compatible, and validated in CI. Promote schema evolution playbooks and automated compatibility checks inspired by ByteByteGo’s system design catalog [https://github.com/ByteByteGoHq/system-design-101].
- **Security and compliance by default.** Enforce centralized identity, least-privilege IAM policies, encrypted data at rest/in transit, audit trails, and automated secret rotation.
- **Developer velocity with paved roads.** Provide ready-to-use service templates, standardized pipelines, and shared infra modules so feature teams focus on domain logic rather than reinventing platform scaffolding.

## Target Architecture

### Platform Backbone
- **Ingress & API Gateway:** Deploy a global gateway (e.g., AWS API Gateway + Lambda Authorizer or Kong on ECS) to harmonize REST/GraphQL ingress, attach rate limits, and expose unified observability.
- **Event Gateway:** Harden the Node.js gateway as the canonical producer entry point. Add schema-aware filtering, dead-letter queues (Amazon SQS), and event replay endpoints.
- **Streaming Fabric:** Standardize on Kafka topics per domain with naming conventions (`context.event.vN`) and consumer groups per service. Enable tiered storage for long-term retention.
- **Workflow Orchestration:** Introduce a lightweight orchestrator (Temporal or AWS Step Functions) for long-running sagas such as bet settlement payouts and finance reconciliation.

### Domain Services
- **Scheduling (`saturday_league_football`):** Extract read-heavy endpoints into a dedicated read model (e.g., PostgreSQL logical replication + read replicas or ElasticSearch for search-heavy features). Publish normalized events (`match.scheduled`, `match.finalized`) through the platform gateway.
- **Betting (`sarradabet`):** Split API and web into separate deployable units. Back the API with horizontal scaling (ECS/Fargate or Kubernetes) and attach Redis for odds caching and idempotent wager processing. Consume `match.*` events, apply odds adjustments, emit `bet.market.*` events.
- **Operations Finance (`pickup-game-manager`):** Introduce asynchronous processors for payment and attendance events, aggregate into a ledger store optimized for analytics (e.g., Snowflake/BigQuery ingestion). Implement integration with external payment providers through webhooks handled by dedicated adapters.
- **Experience Web:** Build a BFF (Backend for Frontend) layer that composes data from Scheduling, Betting, and Finance via event-backed read models (Materialized views using DynamoDB/Elasticache). Deploy on the same gateway for caching and localization.

### Data & Analytics Layer
- **Operational Datastores:** Maintain service-owned PostgreSQL instances with automated backups, PITR, and encrypted connections. Standardize migrations with managed pipelines.
- **Caching:** Adopt Redis or DynamoDB DAX for low latency lookups (leaderboards, bet odds). Ensure cache invalidation ties to event deliveries.
- **Data Lake & Warehouse:** Stream all events into an S3-based lake via Kafka Connect, land in BigQuery/Snowflake for BI. Provide dbt models to standardize metrics.
- **Search & Discovery:** Consider OpenSearch for searching players/teams, hooking into event streams for indexing.

### Cross-Cutting Capabilities
- **Identity & Access:** Centralize user identity in the Platform context using Cognito/Auth0. Federation issues downstream per service through JWT or OPA policies.
- **Observability:** Deploy OpenTelemetry collectors sidecar to services, export traces to AWS X-Ray or Grafana Tempo; standardize dashboards + SLO runbooks.
- **Configuration & Secrets:** Adopt AWS Parameter Store / HashiCorp Vault for secrets, integrate with CI pipelines for provisioning.
- **Compliance & Audit:** Implement event-sourced audit logs, align with data retention policies, and embed automated compliance checks in CI/CD.

## Key Domain Event Flows
- **Match lifecycle:** Scheduler publishes `match.scheduled` → Betting seeds odds and Experience Web updates read models. Post-match, `match.finalized` triggers betting settlement (`bet.settled`, `wager.paid`) and analytics ingestion.
- **Bet placement:** Fans place wagers → Betting API validates, writes to Postgres, emits `wager.accepted` → Finance ingests into ledger → Notifications service (future) informs users.
- **Payment reconciliation:** Operations Finance records `payment.received` → Analytics process aggregates → Identity updates user status for perks → Scheduling unlocks priority booking.

## Execution Roadmap

### Phase 0 – Alignment & Foundations
- Audit current deployments, infrastructure spend, and service health metrics.
- Establish architecture working group, publish decision log, and align OKRs around reliability and velocity.

### Phase 1 – Platform Hardening
- Stand up centralized API gateway with auth, quotas, and routing.
- Expand event gateway with DLQ/replay, contract validation in CI, and observability instrumentation.
- Provision shared Redis cluster and Kafka Connect pipelines for lake ingestion.

### Phase 2 – Domain Modernization
- Refactor Scheduling service to emit enriched events and create read replicas.
- Introduce betting odds cache, idempotency keys, and automated bet settlement workflows.
- Add async processors to Pickup Game Manager for ledger publishing and analytics exports.
- Launch BFF for Experience Web backed by event-driven read models.

### Phase 3 – Analytics & Ops Excellence
- Deploy data warehouse, dbt transformations, and standardized KPIs.
- Roll out SLO-based alerting, runbooks, and chaos testing exercises.
- Automate compliance reports, audit trails, and cost governance dashboards.

### Phase 4 – Optimization & Growth
- Evaluate multi-region readiness for critical services.
- Introduce feature flags, progressive delivery, and A/B experimentation framework.
- Expand partner integrations (ticketing, payment providers) through gateway adapters.

## Adoption Checklist
- [ ] Services emit and consume events through the validated gateway with contract tests in CI.
- [ ] API gateway enforces authentication, rate limiting, and structured logging.
- [ ] Each service has automated build, test, deploy pipelines with infrastructure-as-code.
- [ ] Observability stack captures metrics, logs, traces, and defines SLOs/runbooks.
- [ ] Data lake & warehouse ingestion operational with lineage tracked via dbt.
- [ ] Security posture (secrets, IAM, encryption) documented and audited quarterly.
- [ ] Quarterly architecture reviews to recalibrate principles and roadmap priorities.


