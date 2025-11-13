# Runbook – Event Backbone Outage

## Detection
- Alerts: `KafkaClusterUnhealthy`, `SchemaRegistryLatencyHigh` in Grafana/CloudWatch.
- Symptoms: Producers returning 5xx from gateway, consumer lag spiking, schema validation failures.

## Immediate Actions
1. Confirm broker health via `aws kafka describe-cluster` and CloudWatch metrics.
2. If brokers unreachable, fail traffic to standby cluster (update DNS alias `kafka.dev.platform`).
3. Enable gateway circuit breaker mode (`GATEWAY_READ_ONLY=true`) to queue requests without publishing.
4. Notify `#platform-alerts` and declare incident (IC + comms lead).

## Mitigation Steps
- Restart unhealthy brokers via AWS Console or CLI.
- If schema registry latency high, scale Glue registry endpoint or clear cache (invalidate via gateway admin API – TODO implement).
- Validate consumer group rebalancing by checking `kafka-consumer-groups.sh --describe`.

## Post-Recovery
- Disable gateway read-only mode.
- Run smoke tests for each producer (payment, match, bet flows).
- Capture timeline + remediation in `docs/platform/postmortems/<date>-event-backbone.md` within 3 business days.

## Contacts
- Platform On-Call: `@platform-ic`
- AWS Support Case Template: `support/aws/event-backbone.md` (to be added)
