# Service Discovery Usage Guide

## Overview

Consul provides service discovery, health checking, and configuration management for SarradaHub microservices.

## Service Registration

Services automatically register with Consul on startup when `CONSUL_ENABLED=true` is set.

### Registration Process

1. Service starts and connects to Consul
2. Service provides:
   - Service name
   - Service address and port
   - Health check endpoint
   - Tags (optional)
3. Consul monitors service health
4. Unhealthy services are automatically deregistered

### Service Names

- `pickup-game-manager`
- `sarradabet-api`
- `saturday-league-api`
- `identity-service`

## Service Discovery

### HTTP API

Query services via Consul HTTP API:

```bash
# Get all healthy instances of a service
curl http://localhost:8500/v1/health/service/sarradabet-api?passing

# Get service catalog
curl http://localhost:8500/v1/catalog/services
```

### DNS

Services are accessible via DNS:

```
sarradabet-api.service.consul
saturday-league-api.service.consul
identity-service.service.consul
```

### In Code

#### Node.js/TypeScript

```typescript
import { consulService } from './services/consul.service';

const serviceUrl = await consulService.discoverService('sarradabet-api');
```

#### Ruby/Rails

```ruby
service_url = ConsulService.discover_service('sarradabet-api')
```

## Health Checks

Services must provide health check endpoints:

- `/health`: Basic health check (service is running)
- `/ready`: Readiness check (includes dependencies)

Consul checks these endpoints periodically:
- **Interval**: 10 seconds
- **Timeout**: 5 seconds
- **Deregister**: After 30 seconds of failure

## Key-Value Store

Store configuration in Consul KV:

```bash
# Store circuit breaker configuration
curl -X PUT http://localhost:8500/v1/kv/config/circuit-breaker/sarradabet-api \
  -d '{"errorThreshold": 50, "timeout": 5000}'

# Retrieve configuration
curl http://localhost:8500/v1/kv/config/circuit-breaker/sarradabet-api?raw
```

## Service Tags

Services can be tagged for filtering:

- `api`: API services
- `v1`: API version
- `rails`: Rails services
- `nodejs`: Node.js services

Query by tags:

```bash
curl http://localhost:8500/v1/health/service/sarradabet-api?tag=api
```

## Consul UI

Access Consul web UI at: http://localhost:8500

Features:
- View service catalog
- Check service health
- Browse key-value store
- View service instances

## Best Practices

1. **Always check service health** before making requests
2. **Use service discovery** instead of hardcoded URLs
3. **Handle service unavailability** gracefully
4. **Monitor service registration** in logs
5. **Use tags** for service filtering

## Troubleshooting

### Service Not Registering

1. Check `CONSUL_ENABLED=true` is set
2. Verify Consul is running: `curl http://localhost:8500/v1/status/leader`
3. Check service logs for registration errors
4. Verify network connectivity to Consul

### Service Not Discoverable

1. Check service is registered: `curl http://localhost:8500/v1/catalog/service/<service-name>`
2. Verify service health: `curl http://localhost:8500/v1/health/service/<service-name>?passing`
3. Check service health endpoint is accessible
4. Review Consul logs

### Health Check Failing

1. Verify health endpoint returns 200 OK
2. Check endpoint includes all dependencies
3. Review service logs for errors
4. Verify database connections

