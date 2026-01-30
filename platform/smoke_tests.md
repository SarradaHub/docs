# Smoke Test Scenarios

## 1. Match → Bet → Frontend Stream
1. Create a match in Saturday League API (`POST /api/v1/matches`).
2. Verify gateway logs show `scheduling.match.scheduled.v1` accepted.
3. Confirm SarradaBet API exposes new market (`GET /api/v1/bets?matchId=<platform_uid>`).
4. Load `saturday-league-football-frontend` ticker; ensure new event appears within 30s.

## 2. Payment → Ledger
1. Mark payment as paid in Pickup Game Manager.
2. Verify `finance.payment.received.v1` event in Kafka topic and schema registry passes validation.
3. Check downstream ledger consumer (to-be-implemented) receives event.

## 3. Betting Flow
1. Place vote in SarradaBet front-end.
2. Confirm `betting.wager.accepted.v1` event published; ensure ledger topic receives normalized transaction.

## 4. Resilience Checks
- Shut down one gateway task; ensure second instance handles load.
- Pause Kafka consumer to purposely increase lag; verify alarm triggers.

## Validation Tools
- `kafka-console-consumer` or `kafkacat` for topic verification.
- Grafana dashboard for latency and error rates.
- CloudWatch logs for schema validation messages.
