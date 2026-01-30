# Observability Playbook

## Dashboards
- **Gateway Overview** (Grafana): latency, request volume, validation failures, DLQ counts.
- **Kafka Health**: controller count, ISR shrink events, consumer lag (Prometheus scrape via MSK exporter).
- **Consumer Services**: Render API metrics (CPU/memory), Rails job queue depth, Supabase connection usage.

## Metrics Sources
- **Prometheus (AMP)**: scrape OpenTelemetry collector and Kafka exporter. Push custom app metrics via OTLP.
- **CloudWatch**: MSK metrics, ALB metrics, Lambda (if used). Alerts bridged into Grafana.
- **Vercel / Render**: export logs via webhook to centralized collector (Datadog optional).

## Logging
- Structured JSON logs shipped to CloudWatch log groups provisioned by Terraform.
- Set retention to 30 days (dev/staging) and 90 days (prod by override).
- Implement log metrics for recurring errors (e.g., schema validation failure) and alert thresholds.

## Alerting
| Alert | Source | Threshold | Action |
| --- | --- | --- | --- |
| Kafka Active Controller < 1 | CloudWatch | Immediate | Auto page on-call; failover cluster if persists > 5 min. |
| Gateway 5xx Rate > 1% | Prometheus | 5-minute window | Investigate recent deployments; enable read-only mode if needed. |
| Consumer Lag > 1,000 | Prometheus | 10-minute window | Scale consumers or investigate stuck messages. |

Alerts route to Slack `#platform-alerts` and PagerDuty (critical). Document runbooks in `docs/platform/runbooks/`.

## Tracing
- Adopt OpenTelemetry SDKs in services; send spans to collector (role `platform-otel-collector`).
- Enable distributed tracing for event flows (match scheduled → bet created → frontend SSE) with trace IDs embedded in events.

## Governance
- Review dashboards monthly; update SLO targets quarterly.
- Capture incident retrospectives in `docs/platform/postmortems`.
