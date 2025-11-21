# API Gateway Routing Guide

## Overview

The API Gateway (Traefik) routes all requests to appropriate backend services based on path patterns.

## Routing Rules

### Identity Service Routes

- `POST /api/v1/auth/register` → Identity Service
- `POST /api/v1/auth/login` → Identity Service
- `POST /api/v1/auth/validate` → Identity Service (internal)
- `GET /api/v1/users/*` → Identity Service

### Pickup Game Manager Routes

- `GET /api/v1/athletes` → Pickup Game Manager
- `GET /api/v1/matches` → Pickup Game Manager
- `GET /api/v1/payments` → Pickup Game Manager
- `GET /api/v1/incomes` → Pickup Game Manager
- `GET /api/v1/expenses` → Pickup Game Manager

### SarradaBet API Routes

- `GET /api/v1/bets` → SarradaBet API
- `GET /api/v1/categories` → SarradaBet API
- `GET /api/v1/votes` → SarradaBet API
- `GET /api/v1/admin/*` → SarradaBet API

### Saturday League Football API Routes

- `GET /api/v1/championships` → Saturday League API
- `GET /api/v1/rounds` → Saturday League API
- `GET /api/v1/matches` → Saturday League API
- `GET /api/v1/teams` → Saturday League API
- `GET /api/v1/players` → Saturday League API
- `GET /api/v1/player_stats` → Saturday League API

### Health Check Routes

- `GET /api/v1/pickup-game-manager/health` → Pickup Game Manager
- `GET /api/v1/sarradabet/health` → SarradaBet API
- `GET /api/v1/saturday-league/health` → Saturday League API

## Authentication

Most routes require authentication via the `auth-identity` middleware:

1. Client includes JWT token in `Authorization: Bearer <token>` header
2. Gateway forwards token to Identity Service for validation
3. Identity Service returns user context
4. Gateway adds user headers (`X-User-Id`, `X-User-Email`, `X-User-Role`)
5. Request forwarded to backend service

### Public Routes (No Auth Required)

- `POST /api/v1/auth/register`
- `POST /api/v1/auth/login`
- `GET /api/v1/*/health` (health checks)

## Rate Limiting

Each service has configured rate limits:

- **Pickup Game Manager**: 100 req/min (burst: 50)
- **SarradaBet API**: 200 req/min (burst: 100)
- **Saturday League API**: 150 req/min (burst: 75)
- **Identity Service**: 50 req/min (burst: 25)

## CORS

CORS is enabled for:
- http://localhost:3000
- http://localhost:3002
- http://localhost:5173
- http://localhost:8080

## Configuration

Gateway configuration files:

- `platform/api-gateway/traefik.yml`: Static configuration
- `platform/api-gateway/dynamic/routes.yml`: Service routing rules
- `platform/api-gateway/dynamic/middlewares.yml`: Middleware definitions

## Adding New Routes

1. Add route definition to `dynamic/routes.yml`
2. Configure middleware (auth, rate limiting, etc.)
3. Restart Traefik or wait for file watch to reload

## Troubleshooting

### Route Not Working

1. Check route definition in `dynamic/routes.yml`
2. Verify service is registered in Consul
3. Check Traefik logs: `docker logs api-gateway`
4. Verify service health endpoint is accessible

### Authentication Failing

1. Check Identity Service is running
2. Verify token format in request
3. Check Identity Service logs
4. Verify gateway can reach Identity Service

### Rate Limiting Issues

1. Check rate limit configuration in `dynamic/middlewares.yml`
2. Review rate limit headers in response
3. Adjust limits if needed

