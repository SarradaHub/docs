# Discovery & Alignment Inventory

## Project Overview

### Pickup Game Manager
- **Purpose**: Manage pickup sports matches, attendance, and financials with dashboards.
- **Core Domains**: Athletes, Matches, Payments, Incomes, Expenses, Transaction Categories.
- **Key Services**: `FinancialSummary.period_summary`, `EquilibriumPoint.calculate_equilibrium_point` for financial analytics.
- **APIs & Routes**: RESTful CRUD endpoints (`/athletes`, `/matches`, `/payments`, `/incomes`, `/expenses`), dashboard at `/dashboard`, health check at `/up`.
- **Tech Stack**: Ruby on Rails 8, Ruby 3.3, PostgreSQL, Docker Compose, Turbo/Stimulus, RSpec.
- **Data Storage**: PostgreSQL (`pickup_game_manager_*` databases) with multiple logical DBs for cache/queue/cable in production.
- **Deployment**: Kamal-based container deployment with `.env` configuration.
- **Observability & QA**: Brakeman, RubyCritic, RuboCop, RSpec suites.

### SarradaBet
- **Purpose**: Mock betting platform with real-time odds, categories, voting, and admin tooling.
- **Core Domains**: Bets, Odds, Votes, Categories, Admins, AdminActions.
- **Key Services**:
  - Backend: Express.js services per module (`apps/api/src/modules/*`) with Prisma repositories and Zod validation.
  - Frontend: React app consuming API via typed services (`apps/web/src/services`).
- **APIs & Routes**: REST API at `http://localhost:3001/api/v1` covering bets, categories, votes plus health check (`/health`). API key security.
- **Tech Stack**: Node.js/Express (TypeScript), React + Vite frontend, Prisma ORM, PostgreSQL, Turborepo monorepo, Docker.
- **Data Storage**: PostgreSQL with Prisma schema (`apps/api/prisma/schema.prisma`), includes `pg_trgm` extension.
- **Deployment**: GitHub Actions CI/CD, Docker Compose for dev/prod, npm scripts.
- **Observability & QA**: Winston structured logging, request IDs, Jest/Supertest, Vitest, coverage badges.

### Saturday League Football API
- **Purpose**: Football league management API handling championships, teams, players, rounds, stats.
- **Core Domains**: Championships, Teams, Players, Matches, Rounds, PlayerStats.
- **Key Services**:
  - Service objects (`app/services`) for workflows like `Players::AddToRound`, `PlayerStats::BulkUpsert`.
  - Query objects for eager loading domain aggregations.
  - Presenters/Serializers for API responses (`app/presenters`, `app/serializers`).
- **APIs & Routes**: REST API under `app/controllers/api/v1` delivering JSON via presenters; Jbuilder views in `app/views/api/v1`.
- **Tech Stack**: Ruby on Rails (API mode), PostgreSQL, RSpec, Docker.
- **Data Storage**: PostgreSQL with multi-DB setup for cache/queue/cable in production.
- **Deployment**: Docker, Procfile for dev servers.
- **Observability & QA**: Docs highlight need for improved testing; baseline audit notes low coverage and pending tooling.

### Saturday League Football Frontend
- **Purpose**: React frontend (Vite template) intended to consume the Saturday League Football API.
- **Core Domains**: UI components for league dashboards (to be expanded).
- **Key Services**: React hooks and services under `src/` (currently minimal scaffold).
- **APIs & Routes**: Planned consumption of API at configurable `VITE_API_URL`.
- **Tech Stack**: React + TypeScript + Vite, Tailwind CSS.
- **Data Storage**: Browser storage only (state management TBD).
- **Deployment**: Vite build outputs to `dist/`; ready for static hosting.
- **Observability & QA**: ESLint/Vitest scaffolding.

## Cross-Project Touchpoints & Opportunities
- **Shared Entities**: Users/Players (athletes, bettors, league players) and financial transactions (payments, bets) can benefit from a unified identity and ledger service.
- **Event Candidates**:
  - `user.created` / `user.updated` (central identity provider feeding all systems).
  - `match.scheduled`, `match.completed` from sports apps to trigger betting market creation.
  - `payment.received` from Pickup Game Manager aligning with financial reconciliation in shared ledger.
  - `bet.market.created`, `bet.resolved` from SarradaBet to notify frontends and analytics services.
- **Integration Touchpoints**:
  - Saturday League API can publish match events to enable automatic bet market generation in SarradaBet.
  - Pickup Game Manager financial summaries can feed shared analytics dashboards alongside betting revenue.
  - Frontends can subscribe to event streams (via websockets or SSE) exposed by the event gateway for live updates.

## Stakeholders & Ownership
- **Domain Leads**:
  - Pickup Game Manager: Operations & Finance stakeholders.
  - SarradaBet: Betting product and compliance stakeholders.
  - Saturday League API: Sports operations team.
  - Saturday League Frontend: UX/design team.
- **Platform Team**: New cross-functional squad responsible for event backbone, shared services, and governance.
- **Communication Cadence**: Weekly integration sync, shared architecture decision record (ADR) process, incident channel.

## Integration Objectives & KPIs
- **Objectives**:
  1. Unify customer identity across all properties.
  2. Enable automated creation of betting markets from league events.
  3. Consolidate financial metrics from games and betting into a single analytics surface.
  4. Provide real-time updates to web frontends via event streaming.
- **KPIs**:
  - Time to propagate league match update to betting market (<2 minutes initially, target <30 seconds).
  - Percentage of financial events captured in unified ledger (>95%).
  - System uptime for event backbone (>99.5%).
  - Mean time to detect schema breaking changes (<15 minutes via contract tests).

## Risks & Assumptions
- Existing codebases rely on PostgreSQL; event backbone must bridge relational data with streaming consistency.
- Only SarradaBet has mature CI/CD; other projects require pipeline upgrades to enforce event contracts.
- Identity unification may require data migration and reconciliation beyond current scope; placeholder federation assumed.
- Latency constraints for real-time betting may necessitate additional caching or websocket infrastructure downstream.
