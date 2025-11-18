# MongoDB Developer Plugin - Quick Start Guide

Get started with MongoDB in under 30 minutes! 🚀

## 5-Minute Setup

### 1. Create MongoDB Atlas Account (Free)
```bash
Visit: https://www.mongodb.com/cloud/atlas
Sign up with Google or email
Create Organization & Project
```

### 2. Deploy Free Cluster
```
- Click "Deploy Database"
- Choose M0 Sandbox (FREE, 512MB)
- Select region nearest you
- Wait 2-3 minutes for cluster to start
```

### 3. Create Database User
```
- Security → Database Access
- Add Database User
- Username: devuser
- Password: Generate secure password
- Role: readWrite
- Create User
```

### 4. Setup Network Access
```
- Security → Network Access
- Add IP Address
- Choose your location (or Allow 0.0.0.0/0 for now)
- Confirm
```

### 5. Get Connection String
```
- Click "Connect" button
- Choose "Drivers"
- Select Node.js (or your language)
- Copy connection string

Format: mongodb+srv://username:password@cluster.mongodb.net/database_name
```

### 6. Test Connection
```javascript
const { MongoClient } = require('mongodb');

const client = new MongoClient('mongodb+srv://devuser:PASSWORD@cluster.mongodb.net/testdb');

async function test() {
  try {
    await client.connect();
    console.log('✅ Connected to MongoDB!');

    const db = client.db('testdb');
    const collection = db.collection('users');

    // Insert test document
    const result = await collection.insertOne({ name: 'Test User', email: 'test@example.com' });
    console.log('✅ Inserted:', result.insertedId);

    // Find document
    const doc = await collection.findOne({ name: 'Test User' });
    console.log('✅ Found:', doc);

  } finally {
    await client.close();
  }
}

test();
```

## Using the Plugin

### Get Learning Guidance
```
/learn-mongodb

Select your level:
1. Beginner (Week 1-2)
2. Intermediate (Week 3-4)
3. Advanced (Week 5-6)

Get personalized learning path!
```

### Build Queries Interactively
```
/query-builder find

Build your query step by step
```

### Deploy to Production
```
/deployment-guide single-server

Get deployment strategy for your scale
```

### Follow Tutorials
```
/tutorial blog-app

Step-by-step build a blog application
```

## 7 Specialized Agents

### 1️⃣ **Fundamentals**
MongoDB basics, CRUD, Atlas setup

**Use when:** Learning MongoDB from scratch

### 2️⃣ **Queries & Aggregation**
Find operations, aggregation pipeline, analytics

**Use when:** Retrieving and analyzing data

### 3️⃣ **Data Modeling**
Schema design, relationships, patterns

**Use when:** Designing database structure

### 4️⃣ **Performance & Indexing**
Indexes, query optimization, tuning

**Use when:** Speeding up slow queries

### 5️⃣ **Replication & Sharding**
High availability, distributed systems

**Use when:** Scaling to production

### 6️⃣ **Security & Administration**
Authentication, encryption, backups

**Use when:** Securing your database

### 7️⃣ **Application Development**
Drivers, transactions, error handling

**Use when:** Building applications

## 12 Skill Modules

Each skill is deep, practical, and full of code examples:

1. **mongodb-atlas-setup** - Cloud database configuration
2. **mongodb-crud-operations** - Create, Read, Update, Delete
3. **mongodb-find-queries** - Query language and filters
4. **mongodb-aggregation-pipeline** - Data transformation
5. **mongodb-schema-design** - Data modeling patterns
6. **mongodb-index-creation** - Index types and strategy
7. **mongodb-indexing-optimization** - Query performance
8. **mongodb-transactions** - ACID transactions
9. **mongodb-replication-sharding** - High availability
10. **mongodb-authentication** - User authentication
11. **mongodb-security-admin** - Security and encryption
12. **mongodb-app-development** - Production patterns

## Common Tasks

### Task: Insert Data
```javascript
const collection = db.collection('users');

// Insert one
await collection.insertOne({
  name: 'John Doe',
  email: 'john@example.com',
  age: 30
});

// Insert multiple
await collection.insertMany([
  { name: 'Alice', email: 'alice@example.com' },
  { name: 'Bob', email: 'bob@example.com' }
]);
```

**Skill:** mongodb-crud-operations

### Task: Query Data
```javascript
// Find with filter
const users = await collection.find({
  age: { $gte: 18 }
}).toArray();

// Sort and limit
const topUsers = await collection.find({})
  .sort({ createdAt: -1 })
  .limit(10)
  .toArray();
```

**Skill:** mongodb-find-queries

### Task: Update Data
```javascript
// Update one
await collection.updateOne(
  { email: 'john@example.com' },
  { $set: { age: 31 } }
);

// Update many
await collection.updateMany(
  { status: 'inactive' },
  { $set: { status: 'active' } }
);
```

**Skill:** mongodb-crud-operations

### Task: Create Indexes
```javascript
// Index for filtering
await collection.createIndex({ email: 1 });

// Compound index for sorting
await collection.createIndex({ status: 1, createdAt: -1 });

// Unique index
await collection.createIndex({ email: 1 }, { unique: true });
```

**Skill:** mongodb-index-creation

### Task: Aggregation Pipeline
```javascript
const results = await collection.aggregate([
  { $match: { status: 'active' } },
  { $group: { _id: '$city', count: { $sum: 1 } } },
  { $sort: { count: -1 } },
  { $limit: 10 }
]).toArray();
```

**Skill:** mongodb-aggregation-pipeline

### Task: Transactions
```javascript
const session = client.startSession();

try {
  await session.withTransaction(async () => {
    await collection1.insertOne(doc1, { session });
    await collection2.insertOne(doc2, { session });
  });
} finally {
  await session.endSession();
}
```

**Skill:** mongodb-transactions

## Next Steps

### Week 1: Foundations
- [ ] Create MongoDB Atlas account
- [ ] Insert first document
- [ ] Learn query operators
- [ ] Build first CRUD app
- [ ] Run `/learn-mongodb` for guidance

### Week 2: Intermediate
- [ ] Design a schema
- [ ] Create indexes
- [ ] Write complex queries
- [ ] Handle errors
- [ ] Deploy to Atlas M2

### Week 3+: Production
- [ ] Plan high availability
- [ ] Setup security
- [ ] Implement transactions
- [ ] Monitor performance
- [ ] Prepare for scale

## Resources

- **Official Docs:** https://docs.mongodb.com
- **MongoDB University:** https://university.mongodb.com
- **Community Forum:** https://developer.mongodb.com/community

## Troubleshooting

### "IP not whitelisted"
→ Add your IP in Atlas → Security → Network Access

### "Authentication failed"
→ Check username/password match database user

### "Connection timeout"
→ Check firewall, IP whitelist, credentials

### "Query is slow"
→ Use `/query-builder` or create index with `/skills mongodb-index-creation`

---

**You're ready to build with MongoDB! Start with `/learn-mongodb` 🎉**
