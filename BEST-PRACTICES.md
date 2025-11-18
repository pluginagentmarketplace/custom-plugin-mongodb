# MongoDB Best Practices

Master professional MongoDB development with battle-tested practices.

## Design Principles

### ✅ DO: Design for Queries
```javascript
// ✅ Good: Structure for how you'll query
{
  userId: "123",
  name: "John",
  email: "john@example.com",
  createdAt: ISODate(),
  status: "active"
}

// Think: "How will I query this?"
// Then structure accordingly
```

### ✅ DO: Use Appropriate Data Types
```javascript
// ✅ Good: Proper types
{
  name: "John",              // String
  age: 30,                   // Number
  active: true,              // Boolean
  joinedAt: ISODate(),       // Date
  userId: ObjectId("..."),   // ObjectId
  tags: ["tag1", "tag2"],    // Array
}

// ❌ Bad: Everything as strings
{ name: "John", age: "30", active: "true", joinedAt: "2024-01-01" }
```

### ✅ DO: Embed for 1-to-Few
```javascript
// ✅ Good: Embed user address (always accessed together)
{
  _id: ObjectId(),
  name: "John",
  email: "john@example.com",
  address: {
    street: "123 Main St",
    city: "New York",
    zip: "10001"
  }
}
```

### ✅ DO: Reference for 1-to-Many
```javascript
// ✅ Good: Reference orders (unbounded array)
// Users collection
{ _id: ObjectId(), name: "John", email: "john@example.com" }

// Orders collection
{ _id: ObjectId(), userId: ObjectId(), amount: 99.99, ... }
```

## Query Optimization

### ✅ DO: Use Indexes on Filtered Fields
```javascript
// ✅ Create index
db.users.createIndex({ email: 1 })

// ✅ Query uses index (IXSCAN)
db.users.find({ email: "user@example.com" })

// ❌ Without index: Full collection scan (COLLSCAN)
```

### ✅ DO: Use explain() for Every Query
```javascript
// ✅ Always check execution plan
const explain = db.users.find({ email: "..." }).explain("executionStats")

// Check:
// - Stage: IXSCAN (good) or COLLSCAN (bad)
// - nReturned vs totalDocsExamined (should be close)
// - executionTimeMillis (baseline for monitoring)
```

### ✅ DO: Filter Early in Aggregation
```javascript
// ✅ Good: Filter FIRST (reduce documents processed)
db.orders.aggregate([
  { $match: { status: "completed", date: {$gte: startDate} } },
  { $lookup: { ... } },
  { $group: { ... } }
])

// ❌ Bad: Filter LATE (process all documents)
db.orders.aggregate([
  { $lookup: { ... } },
  { $match: { status: "completed" } }  // Too late!
])
```

### ✅ DO: Use Projection to Reduce Data Transfer
```javascript
// ✅ Good: Only fetch needed fields
db.users.find(
  { email: "user@example.com" },
  { projection: { name: 1, email: 1, _id: 0 } }
)

// ❌ Bad: Fetch entire document
db.users.find({ email: "user@example.com" })
```

## Performance

### ✅ DO: Reuse MongoClient Connection
```javascript
// ✅ Good: Single client, connection pool
const client = new MongoClient(uri, { maxPoolSize: 100 })
// Reuse for all operations

// ❌ Bad: New connection each query (slow, resource heavy)
// Don't create new MongoClient every query!
```

### ✅ DO: Use Batch Operations
```javascript
// ✅ Good: Batch inserts
await collection.insertMany([
  { ... }, { ... }, { ... }, ...
])

// ❌ Bad: Insert one by one
for (const doc of docs) {
  await collection.insertOne(doc)  // Slow!
}
```

### ✅ DO: Configure Connection Pools
```javascript
// ✅ Optimal configuration
const client = new MongoClient(uri, {
  maxPoolSize: 100,        // Max connections
  minPoolSize: 10,         // Min keep-alive
  maxIdleTimeMS: 45000,    // Idle timeout
  serverSelectionTimeoutMS: 5000
})
```

## Error Handling

### ✅ DO: Handle All Errors Gracefully
```javascript
// ✅ Good: Proper error handling
try {
  await collection.insertOne({ email: "user@example.com" })
} catch (error) {
  if (error.code === 11000) {
    return res.status(409).json({ message: "Email already exists" })
  } else if (error.name === 'MongoNetworkError') {
    return res.status(500).json({ message: "Database connection failed" })
  } else {
    throw error
  }
}
```

### ✅ DO: Validate Data Before Insert
```javascript
// ✅ Good: Validate first
const { error, value } = schema.validate(data)
if (error) throw error

await collection.insertOne(value)

// ❌ Bad: No validation
await collection.insertOne(untrustedData)
```

### ✅ DO: Use Schema Validation
```javascript
// ✅ Good: Database-level validation
db.createCollection("users", {
  validator: {
    $jsonSchema: {
      required: ["email", "name"],
      properties: {
        email: { bsonType: "string" },
        name: { bsonType: "string" }
      }
    }
  }
})
```

## Security

### ✅ DO: Enable Authentication
```bash
# ✅ Start MongoDB with auth
mongod --auth --dbpath /data/db

# Or in config
security:
  authorization: enabled
```

### ✅ DO: Use Strong Passwords
```javascript
// ✅ Requirements:
// - 12+ characters
// - Mix of upper/lower case
// - Numbers and symbols
// - NOT dictionary words
// - NOT related to username

// Example: P@ssw0rd2024!MongoDB
```

### ✅ DO: Create Separate Database Users
```javascript
// ✅ Good: App-specific user with minimal privileges
db.createUser({
  user: "myapp",
  pwd: "strong_password",
  roles: [{ role: "readWrite", db: "myapp" }]
})

// ❌ Bad: Share credentials, use admin account
```

### ✅ DO: Enable TLS/SSL
```bash
# ✅ Require encryption
mongod --sslMode requireSSL \
  --sslPEMKeyFile /path/to/cert.pem
```

### ✅ DO: Restrict Network Access
```
✅ IP Whitelist: Only specific IPs
✅ VPN: Private network only
✅ Firewall: Restricted ports
❌ 0.0.0.0/0: Anyone on internet
```

## Development Workflow

### ✅ DO: Use Transactions for Multi-Document Operations
```javascript
// ✅ Good: Atomic operations
const session = client.startSession()
try {
  await session.withTransaction(async () => {
    await collection1.insertOne(doc1, { session })
    await collection2.insertOne(doc2, { session })
    // Both succeed or both rollback
  })
} finally {
  await session.endSession()
}
```

### ✅ DO: Test with Realistic Data
```javascript
// ✅ Good: Use production-like data
const testData = generateRealisticData(1000)
await collection.insertMany(testData)

// Run queries and measure performance
const explain = await collection.find({...}).explain("executionStats")
```

### ✅ DO: Monitor Performance in Production
```javascript
// ✅ Track:
// - Query execution time
// - Document count trends
// - Connection pool usage
// - Slow queries (> 100ms)
// - Replication lag (if applicable)
```

## Anti-Patterns to Avoid

### ❌ DON'T: Embed Unbounded Arrays
```javascript
// ❌ Bad: 1000+ comments in one document (hits 16MB limit)
{
  _id: 1,
  title: "Post",
  comments: [/* 1000+ items */]
}

// ✅ Good: Reference instead
// posts: { _id, title, ... }
// comments: { _id, postId, content, ... }
```

### ❌ DON'T: Denormalize Without Purpose
```javascript
// ❌ Bad: Duplicate data without reason
{ _id: 1, name: "John", email: "john@example.com" }
{ _id: 2, userId: 1, name: "John", email: "john@example.com" }  // Duplicate!

// ✅ Good: Denormalize only if frequent queries need it
// Cache computed values, accept update costs
```

### ❌ DON'T: Index Everything
```javascript
// ❌ Bad: 20+ indexes on single collection
db.users.createIndex({ a: 1 })
db.users.createIndex({ b: 1 })
db.users.createIndex({ c: 1 })
// ...

// ✅ Good: Only index frequently filtered/sorted fields
db.users.createIndex({ email: 1, createdAt: -1 })
```

### ❌ DON'T: Query Without Limits
```javascript
// ❌ Bad: No limit on query result
const allUsers = await collection.find({}).toArray()

// ✅ Good: Always use limits
const users = await collection.find({}).limit(100).toArray()
```

### ❌ DON'T: Ignore Connection Pooling
```javascript
// ❌ Bad: New connection each time
async function query() {
  const client = new MongoClient(uri)
  const result = await collection.find({}).toArray()
  await client.close()
  return result
}

// ✅ Good: Reuse client
const client = new MongoClient(uri)
// Query multiple times with same client
```

### ❌ DON'T: Store Passwords as Plain Text
```javascript
// ❌ Bad
{ username: "john", password: "password123" }

// ✅ Good: Hash passwords
const hashedPassword = await bcrypt.hash(password, 10)
{ username: "john", password: hashedPassword }
```

## Monitoring Checklist

- [ ] Query performance monitored (explain plans reviewed)
- [ ] Slow queries identified and optimized
- [ ] Indexes reviewed and unused ones removed
- [ ] Authentication enabled on all databases
- [ ] Backups configured and tested
- [ ] Connection pooling configured
- [ ] Error handling comprehensive
- [ ] Data validation in place
- [ ] Security hardened (TLS, IP whitelist)
- [ ] Growth capacity planned

## Production Readiness

- [ ] Replica set configured (high availability)
- [ ] Backup/restore tested
- [ ] Disaster recovery plan documented
- [ ] Monitoring and alerting enabled
- [ ] Performance baselines established
- [ ] Capacity planning done
- [ ] Security audit completed
- [ ] Load testing passed
- [ ] Documentation written
- [ ] Team trained

---

**Follow these practices to build scalable, secure MongoDB applications!** ⭐
