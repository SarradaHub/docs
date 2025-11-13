# Canonical Event Taxonomy

## Naming & Versioning Conventions
- Event names follow `<domain>.<aggregate>.<action>` (e.g., `match.scheduled`).
- Initial release uses `v1` payloads; version appended as `schemaVersion` in metadata.
- Backward-compatible additions allowed (optional fields). Breaking changes require new version (`schemaVersion: v2`) and dual-publish period.
- Correlation identifiers: `eventId` (UUID), `traceId`, `causationId`, `source` (service name).

## Core Event Families

### Identity
- `user.created`: New platform identity established.
- `user.linked`: External system identifier associated with a platform user.
- `user.verified`: KYC or eligibility check completed.

### Scheduling
- `match.scheduled`: Match created/updated with schedule information.
- `match.finalized`: Match concluded with scores and stats.
- `player.transferred`: Player moves between teams or rosters.

### Betting
- `bet.market.created`: Betting market generated for a match or custom event.
- `bet.market.updated`: Odds or status updated (e.g., suspended, resolved).
- `bet.market.resolved`: Winning odd determined and payouts initiated.
- `wager.accepted`: Individual bet placement recorded.
- `wager.settled`: Bet settled with payout result.

### Operations Finance
- `payment.received`: Payment collected for match attendance or services.
- `payment.refunded`: Payment reversed due to cancellation or refund.
- `attendance.confirmed`: Athlete attendance confirmed with payment status.

### Analytics & Ledger
- `ledger.transaction.recorded`: Normalized financial entry (from betting or operations).
- `metric.ingested`: Aggregated KPI data point captured (e.g., revenue snapshot).

### Experience Web
- `content.highlight.published`: Optional content update for frontends (future).

## Metadata Envelope
All events share a common envelope:
```json
{
  "eventId": "uuid",
  "schemaVersion": "v1",
  "occurredAt": "2025-11-12T10:00:00Z",
  "traceId": "uuid",
  "causationId": "uuid",
  "source": "saturday-league-api",
  "context": {
    "tenantId": "default",
    "environment": "prod"
  },
  "payload": { "...domain fields..." }
}
```

## Contract Repository Structure
```
platform/
└── contracts/
    ├── schemas/
    │   ├── identity/
    │   │   └── user.created.v1.json
    │   ├── scheduling/
    │   │   └── match.scheduled.v1.json
    │   ├── betting/
    │   │   ├── bet.market.created.v1.json
    │   │   └── wager.accepted.v1.json
    │   ├── finance/
    │   │   └── payment.received.v1.json
    │   └── ledger/
    │       └── ledger.transaction.recorded.v1.json
    └── asyncapi/
        └── platform-events.yml
```

## Governance Workflow
1. Propose schema change via pull request in `platform/contracts`.
2. Run contract tests (producer simulators & consumer validators) in CI.
3. Platform architecture review approves/requests changes.
4. Schema registry release triggers notifications to service teams.
5. Update event gateway configuration to enforce new schemas.
