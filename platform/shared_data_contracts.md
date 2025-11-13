# Shared Data Contracts

## Identity Contracts
- **`user.created.v1`** (`platform/contracts/schemas/identity/user.created.v1.json`): Canonical user record containing core identifiers and external references.
- **`user.linked.v1`** *(planned)*: Associates service-specific IDs to the canonical user; contract draft pending identity service build.

## Scheduling Contracts
- **`match.scheduled.v1`** (`platform/contracts/schemas/scheduling/match.scheduled.v1.json`): Broadcasts match lifecycle data with competition context and team info.
- **`match.finalized.v1`** *(planned)*: Will convey final scores, stats, and officiating details once analytics requirements are confirmed.

## Betting Contracts
- **`bet.market.created.v1`** (`platform/contracts/schemas/betting/bet.market.created.v1.json`): Announces market creation with odds array for downstream pricing and display.
- **`wager.accepted.v1`** (`platform/contracts/schemas/betting/wager.accepted.v1.json`): Captures accepted wagers for settlement and ledger consumption.
- **`bet.market.resolved.v1`** *(planned)*: Communicates winning outcomes and settlement window timing.

## Finance Contracts
- **`payment.received.v1`** (`platform/contracts/schemas/finance/payment.received.v1.json`): Normalizes payment events from operations or betting contexts.
- **`payment.refunded.v1`** *(planned)*: Will standardize refunds and chargebacks; schema pending finance policy decisions.

## Ledger & Analytics Contracts
- **`ledger.transaction.recorded.v1`** (`platform/contracts/schemas/ledger/ledger.transaction.recorded.v1.json`): Downstream ledger service publishes normalized accounting entries bound to source events.
- **`metric.ingested.v1`** *(planned)*: Aggregated KPI event to feed analytics sinks; awaiting metric definitions.

## Contract Lifecycle
1. Draft schema in `platform/contracts/schemas/<domain>/<event>.<version>.json`.
2. Reference schema from AsyncAPI spec (`platform/contracts/asyncapi/platform-events.yml`).
3. Run contract validation tests (see `platform/event-gateway/contracts.test.ts` once implemented).
4. Publish version to schema registry; update event gateway routing configs.
5. Deprecate older versions once all consumers acknowledge migration.

## Tooling Roadmap
- Automate schema linting with `ajv` CLI in CI.
- Generate TypeScript types via `json-schema-to-typescript` for producer/consumer SDKs.
- Provide mock data generators for contract testing.
