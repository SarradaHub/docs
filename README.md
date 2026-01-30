# Docs Overview

The `docs` project stores shared knowledge for the SarradaHub ecosystem. It captures platform policies, operational runbooks, architecture notes, and onboarding guidance so that teams can align on the same reference material.

## Structure

- `platform`: Procedures, standards, and background material that apply across services, including governance, deployments, and observability.
- `pickup-game-manager`: Documentation for the Pickup Game Manager application (Rails/RSpec).
- `sarradabet`: Documentation for the SarradaBet betting platform (TypeScript/Express API and React frontend).
- `saturday_league_football`: Documentation for the Saturday League Football backend (Rails/RSpec).
- `saturday_league_football_frontend`: Documentation for the Saturday League Football frontend (React/Vitest).
- `microservices`: Microservices architecture and integration documentation.
- `design-system`: Design system documentation and guidelines.

## Project Documentation

### Pickup Game Manager
- [Testing Guide](./pickup-game-manager/testing.md) - Testing practices and code coverage guidelines

### SarradaBet
- [README](./sarradabet/README.md) - Documentation index
- [API Documentation](./sarradabet/API.md) - Complete API reference
- [Architecture](./sarradabet/ARCHITECTURE.md) - System architecture and design patterns
- [Developer Guide](./sarradabet/DEVELOPER_GUIDE.md) - Development setup and workflow
- [Deployment Guide](./sarradabet/DEPLOYMENT.md) - Production deployment strategies
- [Database Seeding](./sarradabet/DATABASE_SEEDING.md) - Database seeding documentation
- [Web App README](./sarradabet/WEB_APP_README.md) - Web application documentation
- [Testing Guide](./sarradabet/testing.md) - Testing practices and code coverage guidelines

### Saturday League Football
- [Testing Guide](./saturday_league_football/testing.md) - Testing practices and code coverage guidelines
- [Testing & CI](./saturday_league_football/testing_ci.md) - CI/CD testing documentation
- [Architecture Overview](./saturday_league_football/architecture_overview.md) - System architecture
- [Baseline Audit](./saturday_league_football/baseline_audit.md) - Initial project audit
- [Domain Refactors](./saturday_league_football/domain_refactors.md) - Domain refactoring notes
- [Performance & Accessibility](./saturday_league_football/performance_accessibility.md) - Performance and accessibility guidelines

### Saturday League Football Frontend
- [Testing Guide](./saturday_league_football_frontend/testing.md) - Testing practices and code coverage guidelines

## Contributing

- Keep documents concise and actionable.
- Add cross references when content overlaps with material in other repositories.
- When a change affects production process, update the related runbooks or checklists in the same pull request.
- All project-specific documentation should be placed in the corresponding project folder under `docs/`.

