# Migration Guide: Monolith to Microservices

## Overview

This guide helps migrate from the previous architecture to the new microservices architecture.

## Migration Strategy

### Phase 1: Parallel Run

Keep existing direct service calls while building new infrastructure:

1. Deploy API Gateway alongside existing services
2. Deploy Identity Service
3. Deploy Consul
4. Services register with Consul but continue direct calls

### Phase 2: Gradual Migration

Move services one at a time to use new patterns:

1. **Start with new services**: New features use API Gateway
2. **Migrate frontends**: Update to use API Gateway URLs
3. **Migrate APIs**: Update to use service discovery
4. **Remove direct calls**: Replace with gateway/discovery

### Phase 3: Full Migration

All services use microservices patterns:

1. All requests go through API Gateway
2. All service-to-service calls use discovery
3. All authentication via Identity Service
4. Remove legacy code

## Step-by-Step Migration

### Step 1: Update Frontend Services

**Before:**
```typescript
const API_URL = 'http://localhost:8000';
```

**After:**
```typescript
const API_GATEWAY_URL = import.meta.env.VITE_API_GATEWAY_URL || 'http://localhost';
```

### Step 2: Update Authentication

**Before:**
```typescript
// Direct API call
fetch('http://localhost:8000/api/v1/admin/login', {...})
```

**After:**
```typescript
// Via API Gateway to Identity Service
fetch('http://localhost/api/v1/auth/login', {...})
```

### Step 3: Update Service-to-Service Calls

**Before:**
```ruby
# Hardcoded URL
response = HTTParty.get('http://localhost:8000/api/v1/bets')
```

**After:**
```ruby
# Service discovery
service_url = ConsulService.discover_service('sarradabet-api')
response = HTTParty.get("#{service_url}/api/v1/bets")
```

### Step 4: Add Circuit Breakers

**Before:**
```typescript
const response = await axios.get('http://service:port/api');
```

**After:**
```typescript
const response = await circuitBreakerService.callService(
  'service-name',
  'get',
  '/api'
);
```

### Step 5: Add Health Checks

**Before:**
```ruby
# No health check endpoint
```

**After:**
```ruby
# In routes.rb
get 'health', to: 'health#health'
get 'ready', to: 'health#ready'
```

## Environment Variables

### Required for All Services

Add to `.env` files:

```bash
CONSUL_ENABLED=true
CONSUL_URL=http://localhost:8500
IDENTITY_SERVICE_URL=http://localhost:3001
```

### Service-Specific

#### Node.js Services

```bash
USE_IDENTITY_SERVICE=true
SERVICE_API_KEY=<api-key-from-identity-service>
```

#### Rails Services

```bash
CONSUL_ENABLED=true
CONSUL_URL=http://localhost:8500
IDENTITY_SERVICE_URL=http://localhost:3001
```

## Feature Flags

Use feature flags to toggle between old and new:

```ruby
if ENV['USE_IDENTITY_SERVICE'] == 'true'
  # Use Identity Service
else
  # Use legacy auth
end
```

## Rollback Plan

If issues occur:

1. **Disable new features**: Set feature flags to false
2. **Revert to direct calls**: Remove gateway URLs
3. **Use legacy auth**: Disable Identity Service
4. **Monitor**: Check logs and metrics

## Testing Checklist

- [ ] All services register with Consul
- [ ] Health checks are working
- [ ] API Gateway routes correctly
- [ ] Authentication works via Identity Service
- [ ] Service discovery works
- [ ] Circuit breakers prevent failures
- [ ] Monitoring shows service health
- [ ] Frontends use API Gateway

## Common Issues

### Port Conflicts

If ports conflict:
1. Check `start-all.sh` for port assignments
2. Update service ports if needed
3. Update API Gateway routes

### Service Not Found

If service discovery fails:
1. Check Consul is running
2. Verify service registration
3. Check service health
4. Review network connectivity

### Authentication Errors

If auth fails:
1. Verify Identity Service is running
2. Check token format
3. Verify API Gateway can reach Identity Service
4. Check token expiration

## Timeline

Recommended migration timeline:

- **Week 1**: Deploy infrastructure (Gateway, Consul, Identity Service)
- **Week 2**: Migrate frontends
- **Week 3**: Migrate one API service
- **Week 4**: Migrate remaining API services
- **Week 5**: Remove legacy code and test

## Support

For issues during migration:

1. Check service logs
2. Review Consul UI
3. Check API Gateway dashboard
4. Review monitoring dashboards
5. Consult architecture documentation

