# /tutorial - MongoDB Tutorials & Walkthroughs

Follow step-by-step tutorials to learn MongoDB hands-on.

## Description

Interactive tutorials covering common MongoDB use cases and operations.

## Usage

```
/tutorial [topic]
```

## Available Tutorials

### Tutorial 1: Your First MongoDB Database (30 mins)
```
/tutorial first-database

Topics:
1. Create MongoDB Atlas account
2. Create your first cluster
3. Create a database and collection
4. Insert your first document
5. Query documents

Expected Outcome:
- MongoDB Atlas account setup
- Running cluster
- First data inserted and queried
```

### Tutorial 2: Building a Blog Application (2 hours)
```
/tutorial blog-app

Steps:
1. Design schema
   - Users collection
   - Posts collection
   - Comments collection

2. Create indexes
   - On userId for user posts
   - On postId for comments
   - On createdAt for sorting

3. Implement operations
   - Create user
   - Create blog post
   - Add comments
   - Find posts by author
   - Find recent posts

4. Build queries
   - Posts with comment count
   - User with all their posts
   - Popular posts ranking

Complete Code Examples:
- Node.js implementation
- Python implementation
- Shell queries

Time Investment: 2 hours
Difficulty: Beginner
Files: blog-app.zip
```

### Tutorial 3: MongoDB Aggregation Mastery (3 hours)
```
/tutorial aggregation-mastery

Lessons:
1. Aggregation Pipeline Basics
   - $match stage
   - $project stage
   - Simple aggregation

2. Advanced Stages
   - $group and aggregation functions
   - $sort and $limit
   - $lookup for joins

3. Data Transformation
   - $unwind for arrays
   - $addFields for computed fields
   - $facet for multiple pipelines

4. Real-World Examples
   - Sales analytics
   - User engagement metrics
   - Time series data
   - Search results ranking

Hands-On Exercises:
- Exercise 1: Simple aggregation
- Exercise 2: Complex pipeline
- Exercise 3: Real data analysis

Time Investment: 3 hours
Difficulty: Intermediate
Dataset: Public MongoDB sample data
```

### Tutorial 4: Indexing for Performance (2 hours)
```
/tutorial indexing-performance

Topics:
1. Index Types
   - Single field indexes
   - Compound indexes
   - Text indexes
   - Geospatial indexes

2. Index Design
   - ESR rule (Equality, Sort, Range)
   - Index selectivity
   - Avoiding index intersections

3. Query Analysis
   - Using explain()
   - Understanding execution plans
   - IXSCAN vs COLLSCAN

4. Optimization
   - Identifying slow queries
   - Creating optimal indexes
   - Monitoring index usage

Practical Exercises:
- Analyze slow query
- Design optimal index
- Measure performance improvement

Time Investment: 2 hours
Difficulty: Intermediate
```

### Tutorial 5: Replica Sets & High Availability (3 hours)
```
/tutorial replica-sets

Chapters:
1. Concepts & Architecture
   - Primary/Secondary model
   - Automatic failover
   - Read preferences
   - Replication lag

2. Setup & Configuration
   - Initialize replica set
   - Add members
   - Configure priorities
   - Monitor status

3. Operations
   - Failover behavior
   - Backup from secondary
   - Point-in-time recovery
   - Maintenance procedures

4. Troubleshooting
   - Replication lag diagnosis
   - Recovery from failures
   - Network issues

Hands-On Labs:
- Set up local replica set
- Simulate primary failure
- Recover missing data

Time Investment: 3 hours
Difficulty: Advanced
Prerequisites: Single MongoDB server experience
```

### Tutorial 6: Sharding a Growing Collection (4 hours)
```
/tutorial sharding-strategy

Topics:
1. When & Why to Shard
   - Growth indicators
   - Storage limitations
   - Write scalability

2. Shard Key Selection
   - Cardinality analysis
   - Distribution patterns
   - Avoiding hotspots
   - Compound shard keys

3. Sharding Process
   - Enable sharding
   - Create shard key
   - Chunk migration
   - Balancer configuration

4. Multi-Shard Operations
   - Querying sharded data
   - Aggregation across shards
   - Transactions

5. Monitoring & Maintenance
   - Chunk distribution
   - Balancer status
   - Shard utilization

Lab Exercises:
- Design shard key for dataset
- Monitor chunk migration
- Identify hot shards

Time Investment: 4 hours
Difficulty: Advanced
```

### Tutorial 7: Building a Real-Time Chat App (3 hours)
```
/tutorial realtime-chat

Architecture:
- Users collection
- Conversations collection
- Messages collection
- Change streams for real-time updates

Features:
- Create conversation
- Send messages
- Mark as read
- Real-time notifications

Implementation:
- Node.js + Express
- Socket.io for real-time
- MongoDB change streams
- Authentication

Topics:
1. Data modeling for chat
2. Efficient message queries
3. Change streams setup
4. Socket.io integration
5. Error handling

Code Examples:
- Full working application
- Database queries
- Change stream listeners

Time Investment: 3 hours
Difficulty: Intermediate-Advanced
```

### Tutorial 8: Geospatial Queries (2 hours)
```
/tutorial geospatial-queries

Topics:
1. Geospatial Data Format
   - GeoJSON format
   - 2dsphere index
   - Coordinates format

2. Query Types
   - $near: Find nearby
   - $geoWithin: Find in area
   - $geoIntersects: Intersection
   - Distance calculation

3. Real-World Examples
   - Find nearby restaurants
   - Delivery zone coverage
   - Store locator
   - Location history

4. Performance Optimization
   - Index design
   - Query optimization
   - Accuracy vs performance

Hands-On:
- Create geospatial index
- Query with $near
- Calculate distances
- Optimize geo queries

Time Investment: 2 hours
Difficulty: Intermediate
```

## Quick Start Tutorials

### 5-Minute: Insert Your First Document
```javascript
// 1. Create connection
mongosh 'mongodb+srv://user:pass@cluster.mongodb.net/'

// 2. Select database
use myapp

// 3. Insert document
db.users.insertOne({
  name: 'John Doe',
  email: 'john@example.com',
  age: 30
})

// 4. Query it back
db.users.findOne({ email: 'john@example.com' })

Done! ✓
```

### 10-Minute: Your First Query
```javascript
// Insert multiple documents
db.users.insertMany([
  { name: 'Alice', age: 25, status: 'active' },
  { name: 'Bob', age: 30, status: 'active' },
  { name: 'Charlie', age: 35, status: 'inactive' }
])

// Find active users
db.users.find({ status: 'active' })

// Find users over 28
db.users.find({ age: { $gt: 28 } })

// Count results
db.users.find({ status: 'active' }).count()

Done! ✓
```

### 15-Minute: Your First Aggregation
```javascript
// Group users by status and count
db.users.aggregate([
  { $match: { age: { $gte: 25 } } },
  { $group: {
    _id: '$status',
    count: { $sum: 1 },
    avgAge: { $avg: '$age' }
  }},
  { $sort: { count: -1 } }
])

Done! ✓
```

## Recommended Learning Order

1. **First Database** → Get started
2. **Blog App** → Basic CRUD
3. **Your First Query** → Query syntax
4. **Aggregation Mastery** → Advanced queries
5. **Indexing Performance** → Optimization
6. **Replica Sets** → High availability
7. **Sharding Strategy** → Large scale
8. **Geospatial Queries** → Location features
9. **Real-Time Chat** → Advanced app

## How to Use Tutorials

1. Read the tutorial description
2. Follow step-by-step instructions
3. Copy-paste code examples
4. Run in your MongoDB instance
5. Modify and experiment
6. Complete exercises
7. Move to next topic

## Tips

✅ Type commands yourself (don't just copy)
✅ Understand each step before moving on
✅ Modify examples to experiment
✅ Check results at each step
✅ Ask questions if stuck
✅ Join MongoDB community for help
