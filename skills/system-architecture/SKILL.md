---
name: system-architecture
description: Master system design and architecture patterns. Learn scalability, reliability, microservices, caching, and building systems that stand the test of time. Use when designing systems, optimizing performance, or planning large-scale applications.
---

# System Architecture Skill

Master system design with comprehensive guidance on building scalable, reliable systems.

## Quick Start

### Horizontal Scaling with Load Balancer
```
                    ┌─────────────┐
                    │ Client      │
                    └──────┬──────┘
                           │
                    ┌──────┴──────┐
                    │ Load Balancer│
                    └──────┬──────┘
           ┌────────────────┼────────────────┐
           │                │                │
        ┌──▼──┐          ┌──▼──┐          ┌──▼──┐
        │App-1 │          │App-2 │          │App-3 │
        └──┬───┘          └──┬───┘          └──┬───┘
           │                │                │
           └────────────────┼────────────────┘
                           │
                    ┌──────▼──────┐
                    │ Database    │
                    │(Read Replica)
                    └─────────────┘
```

### Caching Strategy
```python
# Multi-layer caching
from functools import lru_cache
import redis

# Layer 1: In-memory cache (application)
@lru_cache(maxsize=1000)
def expensive_computation(param):
    return compute(param)

# Layer 2: Redis cache (distributed)
def get_cached_data(key):
    redis_cache = redis.Redis()

    # Try Redis first
    cached = redis_cache.get(key)
    if cached:
        return json.loads(cached)

    # Compute if not in cache
    result = expensive_operation()

    # Store in Redis (1 hour TTL)
    redis_cache.setex(key, 3600, json.dumps(result))

    return result

# Layer 3: Database
# Use indexes and query optimization
```

### Microservices Communication
```javascript
// Service-to-Service Communication with Circuit Breaker
class ServiceClient {
    constructor(baseUrl) {
        this.baseUrl = baseUrl;
        this.failures = 0;
        this.lastFailTime = null;
    }

    async request(endpoint) {
        // Circuit breaker logic
        if (this.isCircuitOpen()) {
            throw new Error('Circuit breaker is open');
        }

        try {
            const response = await fetch(`${this.baseUrl}${endpoint}`);
            this.onSuccess();
            return response.json();
        } catch (error) {
            this.onFailure();
            throw error;
        }
    }

    isCircuitOpen() {
        if (this.failures < 5) return false;

        const timeSinceLastFailure = Date.now() - this.lastFailTime;
        return timeSinceLastFailure < 60000; // 1 minute timeout
    }

    onSuccess() {
        this.failures = 0;
    }

    onFailure() {
        this.failures++;
        this.lastFailTime = Date.now();
    }
}
```

### Database Sharding
```
Sharding by User ID (Hash-based):

User 1-1000000   │   User 1000001-2000000   │   User 2000001-3000000
                 │                          │
            ┌────▼────┐               ┌────▼────┐               ┌────▼────┐
            │ Shard 1  │               │ Shard 2  │               │ Shard 3  │
            │ DB-1     │               │ DB-2     │               │ DB-3     │
            └──────────┘               └──────────┘               └──────────┘

Routing logic:
    shard_id = hash(user_id) % num_shards
```

### API Rate Limiting
```python
from time import time
from collections import defaultdict

class RateLimiter:
    def __init__(self, max_requests=100, time_window=60):
        self.max_requests = max_requests
        self.time_window = time_window
        self.requests = defaultdict(list)

    def is_allowed(self, client_id):
        current_time = time()

        # Clean old requests
        self.requests[client_id] = [
            req_time for req_time in self.requests[client_id]
            if current_time - req_time < self.time_window
        ]

        # Check if limit exceeded
        if len(self.requests[client_id]) >= self.max_requests:
            return False

        # Record new request
        self.requests[client_id].append(current_time)
        return True
```

## Advanced Topics

### Scalability Patterns
- Horizontal vs. vertical scaling
- Database replication and sharding
- Caching strategies and invalidation
- Message queues and async processing
- CDN and edge computing

### Reliability Patterns
- Circuit breakers and bulkheads
- Timeout and retry logic
- Health checks and failover
- Graceful degradation
- Load shedding

### Microservices Architecture
- Service decomposition
- API gateways
- Service discovery
- Distributed tracing
- Event-driven communication

### Performance Optimization
- Query optimization
- Connection pooling
- Resource caching
- Compression and encoding
- Monitoring and profiling

### Security Architecture
- Authentication and authorization
- API security
- Data encryption
- Network security
- Audit logging

## Resources

- [System Design Primer](https://github.com/donnemartin/system-design-primer)
- [AWS Architecture Center](https://aws.amazon.com/architecture)
- [Google Cloud Patterns](https://cloud.google.com/architecture)
- [CQRS and Event Sourcing Patterns](https://martinfowler.com/articles/cqrs.html)
