# SarradaHub Microservices Architecture

## Overview

SarradaHub has been transformed into a production-ready microservices architecture. Each service is independently deployable, scalable, and communicates through standardized patterns.

## Architecture Components

### 1. API Gateway (Traefik)

**Location**: `platform/api-gateway/`

The API Gateway serves as the single entry point for all client requests. It provides:

- Request routing to backend services
- Authentication/authorization delegation to Identity Service
- Rate limiting per service
- CORS handling
- SSL/TLS termination
- Health check aggregation

**Access**: 
- HTTP: http://localhost:80
- HTTPS: http://localhost:443
- Dashboard: http://localhost:8080

### 2. Identity Service

**Location**: `platform/identity-service/`

Centralized authentication and authorization service providing:

- User registration and login
- JWT token generation and validation
- API key management for service-to-service auth
- User profile management
- Session management

**Port**: 3001

### 3. Service Discovery (Consul)

**Location**: `platform/service-discovery/`

Provides:

- Service registration and discovery
- Health checking
- Key-value configuration store
- DNS-based service resolution

**Ports**: 
- HTTP API: 8500
- DNS: 8600

**UI**: http://localhost:8500

### 4. Monitoring & Observability

**Location**: `platform/monitoring/`

- **Prometheus**: Metrics collection and storage (port 9090)
- **Grafana**: Visualization and dashboards (port 3001)

### 5. Application Services

#### Pickup Game Manager
- **Port**: 3000
- **Database**: PostgreSQL (port 5432)
- **Routes**: `/api/v1/athletes`, `/api/v1/matches`, `/api/v1/payments`, etc.

#### SarradaBet API
- **Port**: 8000
- **Database**: PostgreSQL (port 5433)
- **Routes**: `/api/v1/bets`, `/api/v1/categories`, `/api/v1/votes`, `/api/v1/admin`

#### Saturday League Football API
- **Port**: 8080
- **Database**: PostgreSQL (port 5434)
- **Routes**: `/api/v1/championships`, `/api/v1/rounds`, `/api/v1/matches`, etc.

#### Frontend Services
- **SarradaBet Web**: Port 3002
- **Saturday League Frontend**: Port 5173

## Communication Patterns

### Synchronous Communication

All services communicate via REST APIs through the API Gateway. Service-to-service calls use:

1. **Service Discovery**: Services discover each other via Consul
2. **Circuit Breakers**: Prevent cascading failures
3. **Identity Service**: Centralized authentication

### Asynchronous Communication

Event-driven communication via Kafka (existing event-gateway) for:
- User events
- Match events
- Bet events
- Payment events

## Service Registration

Each service automatically registers with Consul on startup:

1. Service provides health check endpoints (`/health`, `/ready`)
2. Consul monitors service health
3. Unhealthy services are automatically deregistered

## Authentication Flow

1. Client requests authentication via API Gateway
2. Gateway routes to Identity Service
3. Identity Service validates credentials and returns JWT
4. Client includes JWT in subsequent requests
5. Gateway validates JWT with Identity Service
6. Gateway forwards request to backend service with user context

## Circuit Breaker Pattern

All service-to-service calls are protected by circuit breakers:

- **Node.js services**: Use `opossum` library
- **Rails services**: Use `circuitbox` gem

Circuit breakers:
- Open after N failures
- Half-open after timeout
- Close on successful requests

## Health Checks

All services provide standardized health endpoints:

- `/health`: Basic health check (service is running)
- `/ready`: Readiness check (includes dependencies like database)

## Deployment

Use `start-all.sh` to start all services:

```bash
./start-all.sh
```

Stop all services:

```bash
./start-all.sh stop
```

## Environment Variables

### Required for All Services

- `CONSUL_ENABLED`: Set to "true" to enable service discovery
- `CONSUL_URL`: Consul API URL (default: http://localhost:8500)
- `IDENTITY_SERVICE_URL`: Identity Service URL (default: http://localhost:3001)

### Service-Specific

See individual service README files for detailed environment variable requirements.

## Network Architecture

All services run on a shared Docker network (`sarradahub-network`) enabling:

- Service-to-service communication via service names
- Isolated network for microservices
- Easy service discovery

## Monitoring

### Prometheus Metrics

Services expose metrics at `/metrics` endpoint:
- HTTP request rate
- HTTP error rate
- Response time
- Circuit breaker state

### Grafana Dashboards

Pre-configured dashboards for:
- Service health overview
- Request rates
- Error rates
- Response times

## Best Practices

1. **Always use API Gateway** for external requests
2. **Use service discovery** for service-to-service calls
3. **Implement circuit breakers** for all external dependencies
4. **Monitor health endpoints** for service availability
5. **Use Identity Service** for all authentication needs
6. **Follow API versioning** (`/api/v1/*`)

## Troubleshooting

### Service Not Registering with Consul

1. Check `CONSUL_ENABLED=true` is set
2. Verify Consul is running: `curl http://localhost:8500/v1/status/leader`
3. Check service logs for registration errors

### Circuit Breaker Open

1. Check target service health
2. Review circuit breaker metrics in Prometheus
3. Wait for reset timeout or manually reset

### Authentication Failures

1. Verify Identity Service is running
2. Check JWT token expiration
3. Verify API Gateway can reach Identity Service

