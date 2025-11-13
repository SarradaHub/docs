# Domain Map & Bounded Contexts

## Context Summary

| Context | Owning Team | Core Responsibilities | Primary Data Stores | Key Integrations |
| --- | --- | --- | --- | --- |
| Identity | Platform | Master user registry, identity correlation, API key management | PostgreSQL (`platform_identity`) | Publishes `user.*` events to all services |
| Scheduling | Saturday League API | Manage championships, matches, rounds, player rosters | PostgreSQL (`saturday_league_football_*`) | Consumes `user.*`, publishes `match.*`, `player.*` |
| Betting | SarradaBet API | Bet markets, odds lifecycle, vote tracking, admin controls | PostgreSQL (`sarradabet`) + Redis (future cache) | Consumes `match.*`, `user.*`; publishes `bet.*`, `wager.*` |
| Operations Finance | Pickup Game Manager | Match attendance, payments, incomes, expenses, dashboards | PostgreSQL (`pickup_game_manager_*`) | Consumes `user.*`, publishes `payment.*`, `attendance.*` |
| Experience Web | Saturday League Frontend | Fan-facing UI for schedules, stats, betting links | Browser storage, CDN assets | Consumes `match.*`, `bet.*`, `score.*` via gateway |
| Analytics & Ledger | Platform | Unified BI, revenue tracking, compliance exports | Data warehouse (e.g., BigQuery) + Event lake | Consumes all domain events |

## Context Interactions

```
Identity ── user.created ─► Scheduling
Identity ── user.linked  ─► Operations Finance
Identity ── user.verified ─► Betting

Scheduling ── match.scheduled ─► Betting
Scheduling ── match.finalized ─► Betting, Experience Web, Analytics
Scheduling ── player.transferred ─► Operations Finance

Betting ── bet.market.created ─► Experience Web, Analytics
Betting ── wager.accepted      ─► Analytics & Ledger
Betting ── bet.resolved        ─► Experience Web, Operations Finance, Analytics

Operations Finance ── payment.received ─► Analytics & Ledger
Operations Finance ── attendance.confirmed ─► Scheduling, Experience Web

Experience Web ── fan.feedback.submitted ─► Analytics & Ledger (future)
```

## Aggregates & Ownership Boundaries

- **User** (`Identity`): Global identifier with correlated service-specific IDs (`externalRefs` map). Immutable identifiers; mutable profile details.
- **Match** (`Scheduling`): Aggregate root coordinating teams, players, schedule, outcomes. Emits lifecycle events for other contexts.
- **Bet Market** (`Betting`): Aggregate of bet, odds, votes. Consistency managed internally; external services rely on event updates.
- **Financial Transaction** (`Operations Finance` + `Betting`): Shared schema for ledger entries; each context publishes to `ledger.transaction.recorded` with source metadata.

## Shared Vocabulary

| Term | Definition |
| --- | --- |
| **Event** | Immutable fact published on the event backbone with schema versioning. |
| **Contract** | JSON Schema document defining payload shape; shared via schema registry. |
| **Domain Adapter** | Service-specific component translating internal models into canonical events and vice-versa. |
| **Event Gateway** | Platform service providing ingestion, validation, routing, DLQ, and streaming APIs. |

## Ownership & Governance

- Each context maintains its own codebase but adheres to shared event contracts.
- Platform team curates the canonical schema repo (`platform/contracts`), reviews changes via ADR workflow.
- Schema changes require backward-compatible evolution (additive first), with deprecation policy managed via version metadata.
