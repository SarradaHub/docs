# Environment Variable Matrix

| Service | Hosting Platform | Required Variables | Notes |
| --- | --- | --- | --- |
| Pickup Game Manager | Kamal (Rails) | `DATABASE_URL`, `POSTGRES_HOST`, `EVENT_GATEWAY_URL`, `EVENT_GATEWAY_API_KEY`, `PLATFORM_DEFAULT_CURRENCY`, `RAILS_MASTER_KEY` | Ensure Kamal secrets synced; gateway credentials shared across services. |
| Saturday League Football API | TBD (Rails) | `DATABASE_URL`, `EVENT_GATEWAY_URL`, `EVENT_GATEWAY_API_KEY`, `RAILS_MASTER_KEY` | Add Redis/Sidekiq vars if background jobs enabled. |
| SarradaBet API | Render | `DATABASE_URL` (Supabase), `DIRECT_URL`, `EVENT_GATEWAY_URL`, `EVENT_GATEWAY_API_KEY`, `KAFKA_BROKERS`, `KAFKA_CLIENT_ID`, `KAFKA_CONSUMER_GROUP`, `GLUE_REGISTRY_NAME`, `AWS_REGION`, `JWT_SECRET`, `PORT` | Render deploy hooks must run Prisma migrations. |
| SarradaBet Frontend | Vercel | `NEXT_PUBLIC_API_URL` (or `VITE_API_URL`), `NEXT_PUBLIC_EVENT_STREAM_URL` | Confirm actual variable names in monorepo; align with gateway SSE endpoint. |
| Saturday League Frontend | Vercel | `VITE_BASE_URL`, `VITE_EVENT_STREAM_URL` | Base URL should point to API gateway or BFF. |
| Event Gateway | ECS/Render (target) | `PORT`, `AWS_REGION`, `GLUE_REGISTRY_NAME`, `KAFKA_BROKERS`, `KAFKA_CLIENT_ID`, `KAFKA_SSL`, `KAFKA_USERNAME`, `KAFKA_PASSWORD`, `SCHEMA_CACHE_TTL_MS`, `EVENT_GATEWAY_API_KEY` | Use IAM for MSK/Glue where possible; fallback to SASL user/pass. |

## Secret Distribution Plan
- Centralize secrets in GitHub Environments (`platform-dev`, `platform-staging`, `platform-prod`).
- Use automation workflow to sync to Vercel/Render via API on deployments.
- Maintain `.env.example` files per repo to document expected keys.
