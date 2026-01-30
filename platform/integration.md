# Integration Adapters

## Event Gateway (`platform/event-gateway`)
- Provides REST ingestion (`POST /events/:subject`) and schema validation backed by AWS Glue.
- Publishes validated events to Kafka (MSK) and exposes SSE streams at `/streams/:subject` for web clients.
- Includes Vitest coverage for request handling and an AsyncAPI spec under `platform/contracts/asyncapi`.

## Pickup Game Manager (`pickup-game-manager`)
- `Events::EventClient` posts canonical events to the gateway using env vars `EVENT_GATEWAY_URL` and `EVENT_GATEWAY_API_KEY`.
- `Events::PublishPaymentReceivedJob` serializes `payment.received.v1`; `Payment` model enqueues the job when status transitions to `paid`.
- Spec coverage lives in `spec/jobs/events/publish_payment_received_job_spec.rb`.

## Saturday League Football API (`saturday_league_football`)
- Adds `platform_uid` UUIDs to matches (via migration `20251112090000_add_platform_uid_to_matches.rb`).
- `Events::PublishMatchScheduledJob` emits `match.scheduled.v1` after match creation/update and team assignment.
- Event client shares gateway configuration with pickup manager; tests located at `spec/jobs/events/publish_match_scheduled_job_spec.rb`.

## SarradaBet (`sarradabet`)
- `EventGatewayClient` wraps HTTP publishing; `BetService` publishes `bet.market.created.v1` after bet creation.
- Vote controller emits `wager.accepted.v1` upon vote creation.
- Kafka consumer (`MatchEventConsumer`) listens to `scheduling.match.scheduled.v1`, upserts markets with `externalMatchId`, and seeds default odds via Prisma migration `20251112092000_add_external_match_id_to_bets`.
- Environment schema (`src/config/env.ts`) validates gateway/Kafka variables; dependencies added for `axios` and `kafkajs`.

## Saturday League Frontend (`saturday_league_football_frontend`)
- `useEventStream` hook consumes SSE streams from the gateway (`VITE_EVENT_STREAM_URL`).
- `MatchEventTicker` component surfaces live match scheduling events on the home page.
- README documents new environment variables for developers.

## Configuration Summary
| Service | Env Variables |
| --- | --- |
| Gateway | `PORT`, `AWS_REGION`, `GLUE_REGISTRY_NAME`, `KAFKA_BROKERS`, `KAFKA_USERNAME`, `KAFKA_PASSWORD`, `SCHEMA_CACHE_TTL_MS` |
| Pickup Game Manager | `EVENT_GATEWAY_URL`, `EVENT_GATEWAY_API_KEY`, `PLATFORM_DEFAULT_CURRENCY` |
| Saturday League API | `EVENT_GATEWAY_URL`, `EVENT_GATEWAY_API_KEY` |
| SarradaBet API | `EVENT_GATEWAY_URL`, `EVENT_GATEWAY_API_KEY`, `KAFKA_BROKERS`, `KAFKA_CLIENT_ID`, `KAFKA_CONSUMER_GROUP` |
| Saturday League Frontend | `VITE_BASE_URL`, `VITE_EVENT_STREAM_URL` |

## Next Steps
- Harden gateway auth (API keys + mTLS between platform workloads).
- Expand contract publishing to include `bet.market.resolved` and `match.finalized` schemas.
- Implement end-to-end contract tests within CI using the new schema registry module.
- Extend consumer to reconcile identity events once unified identity service is live.
