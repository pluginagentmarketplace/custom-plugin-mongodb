---
name: database-design
description: Master database design, optimization, and management. Learn SQL, schema design, indexing, NoSQL databases, and scaling strategies. Use when designing databases, writing efficient queries, or optimizing performance.
---

# Database Design Skill

Master database design and optimization with comprehensive guidance on relational and NoSQL databases.

## Quick Start

### PostgreSQL Schema Design
```sql
-- Create schema
CREATE TABLE users (
    id BIGSERIAL PRIMARY KEY,
    email VARCHAR(255) UNIQUE NOT NULL,
    name VARCHAR(255) NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

CREATE TABLE posts (
    id BIGSERIAL PRIMARY KEY,
    user_id BIGINT NOT NULL REFERENCES users(id) ON DELETE CASCADE,
    title VARCHAR(255) NOT NULL,
    content TEXT NOT NULL,
    published BOOLEAN DEFAULT false,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

CREATE TABLE comments (
    id BIGSERIAL PRIMARY KEY,
    post_id BIGINT NOT NULL REFERENCES posts(id) ON DELETE CASCADE,
    user_id BIGINT NOT NULL REFERENCES users(id) ON DELETE CASCADE,
    content TEXT NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Create indexes
CREATE INDEX idx_posts_user_id ON posts(user_id);
CREATE INDEX idx_comments_post_id ON comments(post_id);
CREATE INDEX idx_comments_user_id ON comments(user_id);
```

### MongoDB Document Design
```javascript
// User document
db.users.insertOne({
    _id: ObjectId(),
    email: "user@example.com",
    name: "John Doe",
    profile: {
        bio: "Software developer",
        avatar: "https://...",
        socials: {
            twitter: "@johndoe",
            github: "johndoe"
        }
    },
    preferences: {
        theme: "dark",
        notifications: true
    },
    createdAt: new Date(),
    updatedAt: new Date()
});

// Post with embedded comments
db.posts.insertOne({
    _id: ObjectId(),
    userId: ObjectId("user_id"),
    title: "My First Post",
    content: "...",
    tags: ["mongodb", "nodejs"],
    comments: [
        {
            userId: ObjectId("user_id"),
            text: "Great post!",
            createdAt: new Date()
        }
    ],
    createdAt: new Date()
});
```

### Optimized Queries
```sql
-- Good: Using indexes
SELECT posts.* FROM posts
WHERE user_id = 123
ORDER BY created_at DESC
LIMIT 10;

-- Good: Using EXPLAIN to check execution
EXPLAIN (ANALYZE, BUFFERS)
SELECT * FROM posts WHERE user_id = 123;

-- Bad: Full table scan
SELECT * FROM posts
WHERE EXTRACT(YEAR FROM created_at) = 2024;

-- Good: Use date range
SELECT * FROM posts
WHERE created_at >= '2024-01-01' AND created_at < '2025-01-01';
```

### Redis Caching
```python
import redis
import json

r = redis.Redis(host='localhost', port=6379, db=0)

# Cache user data
def get_user(user_id):
    # Try cache first
    cached = r.get(f'user:{user_id}')
    if cached:
        return json.loads(cached)

    # Fetch from database
    user = fetch_user_from_db(user_id)

    # Store in cache for 1 hour
    r.setex(f'user:{user_id}', 3600, json.dumps(user))

    return user

# Cache invalidation
def update_user(user_id, data):
    update_in_database(user_id, data)
    r.delete(f'user:{user_id}')  # Invalidate cache
```

## Advanced Topics

### Schema Optimization
- Normalization vs. denormalization
- Partitioning and sharding strategies
- Choosing between SQL and NoSQL
- Polyglot persistence

### Query Optimization
- Index design and usage
- Query execution plans
- Join optimization
- Window functions and CTEs

### Scaling Strategies
- Read replicas and replication
- Master-slave vs. multi-master
- Sharding keys and distribution
- Consistency models (ACID, BASE, eventual consistency)

### Backup & Recovery
- Point-in-time recovery
- Incremental vs. full backups
- Disaster recovery planning
- RTO and RPO metrics

### Database Monitoring
- Slow query logs
- Connection pooling
- Performance metrics and alerting
- Capacity planning

## Resources

- [PostgreSQL Documentation](https://www.postgresql.org/docs)
- [MongoDB Documentation](https://docs.mongodb.com)
- [Redis Documentation](https://redis.io/documentation)
- [SQL Performance Explained](https://sql-performance-explained.com)
