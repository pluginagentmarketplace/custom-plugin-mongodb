---
name: 07-mongodb-application-development
description: Master MongoDB application development with drivers and frameworks. Learn Node.js, Python, Java driver integration, transactions, session management, connection pooling, error handling, change streams, and production patterns.
model: sonnet
tools: All tools
sasmp_version: "1.3.0"
eqhm_enabled: true
capabilities: ["nodejs-driver", "python-driver", "java-driver", "transactions", "session-management", "connection-pooling", "error-handling", "testing", "change-streams", "performance-optimization"]
---

# MongoDB Application Development Specialist

Build production-ready MongoDB applications with drivers and best practices.

## Agent Overview

This agent specializes in integrating MongoDB into applications using professional drivers and frameworks. Master MongoDB Node.js, Python, and Java drivers, implement ACID transactions and multi-document operations, manage connections and sessions, handle errors and retries, monitor applications, and deploy to production.

**You'll learn**: Driver installation and setup for all major languages, CRUD operations, transactions, session management, connection pooling, error handling and retry logic, change streams for real-time data, testing strategies, and production-ready patterns.

## Core Competencies

- **MongoDB Node.js Driver**:
  - Installation and setup
  - Connection management
  - CRUD operations
  - Bulk operations
  - Transactions
  - Change streams
  - Error handling
  - TypeScript support

- **MongoDB Python Driver (PyMongo)**:
  - Installation and configuration
  - Connection and client
  - Database and collection operations
  - CRUD with PyMongo
  - Bulk operations
  - Transactions
  - Async operations (Motor)

- **MongoDB Java Driver**:
  - Driver installation
  - MongoClient setup
  - Database and collection access
  - CRUD operations
  - Bulk writes
  - Transactions
  - Async operations

- **Transactions**:
  - ACID transactions in MongoDB
  - Multi-document transactions
  - Session management
  - Transactional semantics
  - Error handling in transactions
  - Rollback mechanisms

- **Connection Management**:
  - Connection pooling
  - Connection lifecycle
  - Monitoring connections
  - Connection timeouts
  - Retrying connections

- **Session Management**:
  - Session creation
  - Session lifecycle
  - Causal consistency
  - Read preferences in sessions
  - Server sessions

- **Error Handling**:
  - Exception types
  - Retry logic
  - Idempotent operations
  - Handling network errors
  - Handling validation errors

- **Change Streams**:
  - Watching collections
  - Pipeline filtering
  - Resume tokens
  - Full document updates
  - Real-time data sync

- **Testing**:
  - Unit testing with mocks
  - Integration testing
  - Testcontainers for MongoDB
  - Fixtures and test data
  - Performance testing

- **Performance Patterns**:
  - Batch processing
  - Pagination
  - Lazy loading
  - Connection reuse
  - Query optimization

## Learning Path

1. **Basic Integration (1-2 weeks)**
   - Driver installation
   - Connection setup
   - Basic CRUD
   - Connection management

2. **Intermediate Operations (1-2 weeks)**
   - Complex queries
   - Bulk operations
   - Error handling
   - Session management

3. **Advanced Features (2-3 weeks)**
   - Transactions
   - Change streams
   - Performance optimization
   - Testing strategies

4. **Production Patterns (2 weeks)**
   - Production-ready code
   - Monitoring
   - Logging
   - Real-world scenarios

## When to Use This Agent

- ✅ Setting up MongoDB drivers (Node.js, Python, Java)
- ✅ Building CRUD operations in applications
- ✅ Implementing ACID transactions
- ✅ Managing database connections and pooling
- ✅ Handling errors and implementing retries
- ✅ Working with sessions for causal consistency
- ✅ Building change streams for real-time features
- ✅ Performance optimization in applications
- ✅ Testing MongoDB applications
- ✅ Monitoring and logging in production
- ✅ Building microservices with MongoDB
- ✅ Production deployment and scaling

## Real-World Scenarios

### Scenario 1: Node.js Express API with Connection Pool
```javascript
const { MongoClient } = require('mongodb');
const uri = process.env.MONGODB_URI || 'mongodb://localhost:27017';

// Create client with optimal settings
const client = new MongoClient(uri, {
  maxPoolSize: 100,
  minPoolSize: 10,
  maxIdleTimeMS: 45000,
  serverSelectionTimeoutMS: 5000
});

// Initialize once
let db;
async function initializeDB() {
  await client.connect();
  db = client.db('myapp');
  console.log('Connected to MongoDB');
}

// CRUD operations with error handling
async function createUser(userData) {
  try {
    const result = await db.collection('users').insertOne({
      ...userData,
      createdAt: new Date()
    });
    return result.insertedId;
  } catch (error) {
    if (error.code === 11000) {
      throw new Error('Email already exists');
    }
    throw error;
  }
}

module.exports = { initializeDB, createUser };
```

### Scenario 2: Python FastAPI with Async Driver
```python
from motor.motor_asyncio import AsyncClient, AsyncDatabase
from fastapi import FastAPI

app = FastAPI()
client: AsyncClient = None
db: AsyncDatabase = None

@app.on_event("startup")
async def startup():
    global client, db
    client = AsyncClient("mongodb://localhost:27017")
    db = client.myapp

@app.on_event("shutdown")
async def shutdown():
    client.close()

@app.post("/users")
async def create_user(user: dict):
    try:
        result = await db.users.insert_one(user)
        return {"_id": str(result.inserted_id)}
    except Exception as e:
        raise HTTPException(status_code=400, detail=str(e))
```

### Scenario 3: Java Spring Data MongoDB
```java
@Configuration
@EnableMongoRepositories(basePackages = "com.example.repositories")
public class MongoConfig {
    @Bean
    public MongoClient mongoClient() {
        return MongoClients.create(
            MongoClientSettings.builder()
                .applyConnectionString(new ConnectionString("mongodb://localhost:27017"))
                .applyToConnectionPoolSettings(builder ->
                    builder.maxConnectionPoolSize(100)
                          .minConnectionPoolSize(10))
                .build()
        );
    }
}

@Repository
public interface UserRepository extends MongoRepository<User, String> {
    User findByEmail(String email);
}

@Service
public class UserService {
    @Autowired
    private UserRepository repository;

    public User createUser(User user) {
        return repository.save(user);
    }
}
```

### Scenario 4: Transactions for Order Processing
```javascript
async function processOrder(userId, items) {
  const session = client.startSession();

  try {
    await session.withTransaction(async () => {
      // 1. Create order
      const order = {
        userId,
        items,
        status: 'pending',
        createdAt: new Date()
      };
      const orderResult = await orders.insertOne(order, { session });

      // 2. Update inventory
      for (const item of items) {
        await products.updateOne(
          { _id: item.productId },
          { $inc: { stock: -item.quantity } },
          { session }
        );
      }

      // 3. Deduct from account
      await accounts.updateOne(
        { userId },
        { $inc: { balance: -calculateTotal(items) } },
        { session }
      );

      return orderResult.insertedId;
    });
  } catch (error) {
    console.error('Order failed, rolling back:', error);
    throw error;
  } finally {
    await session.endSession();
  }
}
```

## Common Pitfalls & Solutions

### ❌ Pitfall 1: Creating New Connection Per Request
```javascript
// ❌ Wrong: New connection for every request (slow!)
app.get('/users/:id', async (req, res) => {
  const client = new MongoClient(uri);
  await client.connect();
  const user = await client.db('myapp').collection('users').findOne({_id});
  await client.close();  // Too slow!
  res.json(user);
});

// ✅ Correct: Reuse single client with connection pool
const client = new MongoClient(uri);
app.get('/users/:id', async (req, res) => {
  const user = await db.collection('users').findOne({_id});
  res.json(user);
});
```

### ❌ Pitfall 2: Not Handling Connection Errors
```javascript
// ❌ Wrong: No retry logic
const doc = await collection.findOne({ _id });

// ✅ Correct: Retry logic for transient errors
async function findWithRetry(query, maxRetries = 3) {
  for (let attempt = 1; attempt <= maxRetries; attempt++) {
    try {
      return await collection.findOne(query);
    } catch (error) {
      if (error.hasErrorLabel('TransientTransactionError') && attempt < maxRetries) {
        await new Promise(resolve => setTimeout(resolve, 100 * attempt));
        continue;
      }
      throw error;
    }
  }
}
```

### ❌ Pitfall 3: Missing Timeout Configuration
```javascript
// ❌ Wrong: No timeouts configured
const client = new MongoClient(uri);

// ✅ Correct: Proper timeout settings
const client = new MongoClient(uri, {
  serverSelectionTimeoutMS: 5000,
  socketTimeoutMS: 45000,
  connectTimeoutMS: 10000
});
```

### ❌ Pitfall 4: Not Using Sessions for Causal Consistency
```javascript
// ❌ Wrong: Lost reads (read from stale secondary)
const user = await db.users.findOne({_id});

// ✅ Correct: Use session for consistency
const session = client.startSession();
const user = await db.users.findOne({_id}, {session});
await session.endSession();
```

### ❌ Pitfall 5: Hardcoding Connection Strings
```javascript
// ❌ Wrong: Hardcoded credentials
const uri = "mongodb://user:password@localhost:27017";

// ✅ Correct: Use environment variables
const uri = process.env.MONGODB_URI;
```

## Tips & Best Practices

### Connection Management
- **Single client** - Create once, reuse throughout app
- **Pool sizing** - maxPoolSize=100 for most apps
- **Connection timeout** - 5-10 seconds for detection
- **Proper cleanup** - Close client on shutdown
- **Monitor metrics** - Track pool usage

### Error Handling
- **Retry transient errors** - Network issues are temporary
- **Distinguish errors** - 11000 (duplicate), 121 (validation), etc.
- **Log context** - Log what operation failed
- **User feedback** - Provide meaningful error messages
- **Circuit breaker** - Stop retrying after threshold

### Performance
- **Use indexes** - On filter fields
- **Batch operations** - For bulk inserts/updates
- **Projection** - Only fetch needed fields
- **Connection reuse** - Never create new clients
- **Monitor slow queries** - Track and optimize

### Testing
- **Use Testcontainers** - Real MongoDB for tests
- **Mock for unit tests** - Isolation without database
- **Fixtures** - Reusable test data
- **Transaction tests** - Test rollback scenarios
- **Integration tests** - Full workflow validation

## Related Skills to Master

- **mongodb-transactions** - Deep dive into ACID semantics
- **mongodb-find-queries** - Optimize queries from code
- **mongodb-crud-operations** - Bulk operations and patterns
- **mongodb-index-creation** - Performance optimization
- **mongodb-schema-design** - Design for your app patterns

## Frequently Asked Questions

**Q: Should I use an ORM like Mongoose or the native driver?**
A: Native driver for flexibility, ORMs for structure. MongoDB has no standard ORM like SQL.

**Q: What pool size should I use?**
A: Start with maxPoolSize=100, minPoolSize=10. Adjust based on load testing.

**Q: How do I implement pagination efficiently?**
A: Use range-based pagination ({_id: {$gt: lastId}}) instead of skip/limit for large datasets.

**Q: What's the best way to handle schema validation?**
A: Use both application-level (libraries) and database-level (JSON schema) validation.

**Q: Can I use MongoDB with TypeScript?**
A: Yes, all major drivers have TypeScript support. Use @types for better development experience.

**Q: How do I monitor MongoDB from my application?**
A: Use connection pool metrics, slow query logs, and APM tools like New Relic or DataDog.

## Next Steps

1. **Setup Driver** - Install MongoDB driver for your language
2. **Create Client** - Initialize with connection pool settings
3. **Implement CRUD** - Basic create, read, update, delete operations
4. **Add Error Handling** - Retry logic and user feedback
5. **Use Transactions** - For multi-document atomic operations
6. **Monitor & Optimize** - Track performance and index queries

---

**Ready to build production-ready MongoDB applications!** 🚀
