# Repository & Deployment Inventory

| Service | GitHub Repository | Primary Stack | Hosting / Deployment Target | Current Delivery Notes |
| --- | --- | --- | --- | --- |
| Saturday League Football Frontend | https://github.com/LMafra/saturday_league_football_frontend | React + Vite | Vercel (`saturday-league-football-frontend.vercel.app`) | Auto-deploys from `main` (to confirm). Needs env vars `VITE_BASE_URL`, `VITE_EVENT_STREAM_URL`. |
| Saturday League Football API | https://github.com/LMafra/saturday_league_football | Rails API | (Likely Kamal or custom deployment) | Requires PostgreSQL + Redis; publishes `match.scheduled`. Gateway configs: `EVENT_GATEWAY_URL`, `EVENT_GATEWAY_API_KEY`. |
| SarradaBet API/Web | https://github.com/LMafra/sarradabet | Node/Express + React (Turborepo) | Render web service (`srv-cvcbu9fnoe9s73calvo0`) + Vercel web front (`sarradabet.vercel.app`) | Render handles API with Supabase Postgres (project `gvoidjhjmidljabjgydb`). Env vars: `DATABASE_URL`, `EVENT_GATEWAY_URL`, `KAFKA_BROKERS`, etc. |
| Pickup Game Manager | https://github.com/LMafra/pickup-game-manager | Rails 8 | Kamal (per `config/deploy.yml`) | Needs Postgres + Redis; emits `payment.received`. Env: `EVENT_GATEWAY_URL`, `EVENT_GATEWAY_API_KEY`, currency defaults. |

## Hosting Notes
- **Vercel**: two projects (`sarradabet`, `saturday-league-football-frontend`). Validate GitHub integration, production/main branch mapping, preview permalinks.
- **Render**: SarradaBet API running on web service `srv-cvcbu9fnoe9s73calvo0`; check autoscaling + health checks.
- **Supabase**: Shared Postgres for SarradaBet; ensure migrations automated via Render deploy hooks.
- **Kamal**: Pickup Game Manager likely deployed via Kamal; audit server inventory and secrets distribution.

## Deployment Triggers & Secrets (to verify)
- Confirm branch protections on each repo (`main` requires PR + status checks).
- Inventory GitHub Actions per repo; align with `platform/ci` workflows.
- Collect environment variables for Vercel/Render/Supabase/Kamal and map to new secrets store.
