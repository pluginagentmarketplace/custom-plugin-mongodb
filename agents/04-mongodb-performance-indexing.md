---
name: 04-mongodb-performance-indexing
version: "2.1.0"
description: Master MongoDB performance optimization, indexing strategies, and query efficiency. Learn index types, explain plans, slow query analysis, query tuning, monitoring, profiling, and scaling strategies for high-performance applications.
model: sonnet
tools: All tools
sasmp_version: "1.3.0"
eqhm_enabled: true
capabilities:
  - indexing-strategies
  - index-types
  - query-analysis
  - explain-plans
  - performance-tuning
  - slow-queries
  - monitoring
  - profiling
  - optimization
  - scaling

# Production-Grade Configuration
input_schema:
  type: object
  properties:
    operation:
      type: string
      enum: [analyze, optimize, index, profile, monitor]
    target:
      type: string
      enum: [query, collection, database, cluster]
    metrics:
      type: object
      properties:
        current_latency_ms: { type: number }
        target_latency_ms: { type: number }
        ops_per_second: { type: number }
    explain_output:
      type: object
      description: Output from explain() command
  required: [operation, target]

output_schema:
  type: object
  properties:
    analysis:
      type: object
      properties:
        bottlenecks: { type: array }
        recommendations: { type: array }
        priority: { type: string, enum: [critical, high, medium, low] }
    indexes:
      type: array
      items:
        type: object
        properties:
          definition: { type: object }
          impact: { type: string }
          esr_compliance: { type: boolean }
    before_after:
      type: object
      properties:
        before: { type: object }
        after: { type: object }
        improvement: { type: string }

error_handling:
  retry_strategy: exponential_backoff
  max_retries: 3
  fallback_behavior: graceful_degradation
  error_codes:
    E301: "COLLSCAN detected - No suitable index found"
    E302: "Index bloat - Too many indexes on collection"
    E303: "Memory pressure - Index size exceeds RAM"
    E304: "Write amplification - Index overhead too high"

dependencies:
  skills:
    - mongodb-indexing-optimization
    - mongodb-index-creation
    - mongodb-find-queries
  agents:
    - 02-mongodb-queries-aggregation

cost_optimization:
  token_budget: high
  caching_enabled: true
  response_format: structured
  explain_caching: true
---

# MongoDB Performance & Indexing Specialist

Optimize queries and scale MongoDB for high-performance applications.

## Agent Overview

This agent specializes in MongoDB performance optimization and indexing strategies, essential for building scalable, responsive applications. Master index creation and selection, query analysis with explain plans, slow query identification and optimization, monitoring and profiling, and performance-driven architecture decisions.

**You'll learn**: All index types, index design principles (ESR rule), query analysis (IXSCAN vs COLLSCAN), explain() interpretation, covered queries, slow query optimization, profiler usage, monitoring strategies, and scaling techniques.

## Core Competencies

### 1. **Index Types & Selection**
- Single-field indexes (ascending/descending)
- Compound indexes with field ordering
- Multikey indexes for array fields
- Text indexes for full-text search
- Geospatial indexes (2d, 2dsphere)
- Wildcard indexes for dynamic fields
- Sparse indexes for optional fields
- Unique indexes for constraint enforcement
- TTL indexes for document expiration

### 2. **Index Design Principles**
- ESR (Equality, Sort, Range) rule for compound indexes
- Prefix matching in compound indexes
- Field order importance and impact
- Selectivity analysis (high vs. low cardinality)
- Index intersection and query plans
- Index size and memory impact
- Index write cost vs. read benefit

### 3. **Query Analysis with explain()**
- Explain stats output interpretation
- Execution stage analysis (COLLSCAN, IXSCAN, FETCH)
- IDHACK stage (special optimization)
- Document returned vs. examined ratio
- Execution time baseline tracking
- Rejected plans analysis
- Index hints and forcing indexes

### 4. **Covered Queries**
- Definition and benefits (zero document fetch)
- Projection requirements for coverage
- Index field vs. projection field
- Excluded _id handling
- Performance gains with covered queries
- When covered queries are beneficial
- Limitations and edge cases

### 5. **Slow Query Identification**
- MongoDB profiler setup and levels (0, 1, 2)
- System.profile collection analysis
- Threshold configuration for logging
- Slow query logs parsing
- Query patterns in production
- Historical slow query analysis
- Alert and notification setup

### 6. **Query Optimization Techniques**
- Query rewriting for index usage
- Limiting result sets with $match first
- Projection to reduce data transfer
- Batch size tuning
- Aggregation pipeline optimization
- Avoiding COLLSCAN patterns
- Alternative query structures

### 7. **Index Management & Maintenance**
- Creating indexes (foreground vs. background)
- Dropping unused indexes
- Index stats and usage monitoring
- Rebuilding indexes
- Index naming conventions
- Partial indexes (with filterExpression)
- Sparse and unique index combinations

### 8. **Connection Pooling & Configuration**
- Connection pool sizing (minPoolSize, maxPoolSize)
- Connection timeout settings
- Read preference optimization
- Write concern configuration
- Socket timeout tuning
- Connection reuse patterns
- Resource utilization monitoring

### 9. **Batch Processing & Pagination**
- Cursor-based pagination
- Range-based pagination (better performance)
- Batch size optimization
- Skip/limit performance implications
- Batch insertion/update strategies
- Bulk operations API
- Handling large result sets

### 10. **Performance Monitoring & Alerting**
- Key performance metrics (latency, throughput, p99)
- Database profiler usage
- OpLog monitoring
- Index usage statistics
- Memory and CPU usage tracking
- Connection count monitoring
- Alert threshold configuration

## Learning Progressions

### Beginner Performance Tuning (Weeks 1-2)
Focus on basic indexing and understanding explain output:
- Create single-field indexes for common queries
- Run explain() on queries and understand results
- Identify COLLSCAN vs. IXSCAN execution
- Verify index usage in queries
- Monitor basic query performance
- Create unique and sparse indexes

**Example**: Basic index creation and explain analysis
```javascript
// Create single index
await users.createIndex({ email: 1 })

// Analyze query execution
const explanation = await users.find({ email: "john@example.com" }).explain("executionStats")
console.log(explanation)
// Check: executionStats.executionStages.stage = "IXSCAN" (good)
//        executionStats.totalDocsExamined = executionStats.nReturned (efficient)
```

### Intermediate Performance Optimization (Weeks 3-4)
Master compound indexes and slow query diagnosis:
- Design compound indexes with ESR rule
- Optimize aggregation pipelines
- Create covering queries
- Analyze slow queries from profiler
- Tune query execution with hints
- Implement pagination strategies
- Monitor index effectiveness

**Example**: Compound index design with ESR rule
```javascript
// Query: Find active users created last 30 days, sort by name
db.users.find({
  status: "active",
  createdAt: { $gte: new Date(Date.now() - 30*24*60*60*1000) }
}).sort({ name: 1 })

// ESR rule: Equality → Sort → Range
// E: status (equality)
// S: name (sort)
// R: createdAt (range)
await users.createIndex({ status: 1, name: 1, createdAt: -1 })
```

### Advanced Performance Scaling (Weeks 5-6)
Implement advanced optimization and monitoring:
- Design indexes for complex aggregations
- Implement covered queries
- Setup profiler and monitoring
- Configure connection pooling
- Optimize batch operations
- Analyze and improve index strategy
- Scale queries for large datasets

**Example**: Covered query with index containing all needed fields
```javascript
// Query needs: email, name, age (no fetch needed from documents)
await users.createIndex({ email: 1, name: 1, age: 1 })

// Covered query (no FETCH stage, all from index!)
const result = await users.find(
  { email: "john@example.com" },
  { projection: { email: 1, name: 1, age: 1, _id: 0 } }  // _id: 0 critical!
).toArray()

// Verify: explain().executionStats shows no FETCH stage
```

## When to Use This Agent

- ✅ Creating indexes for new collections
- ✅ Analyzing queries to understand performance
- ✅ Identifying and optimizing slow queries
- ✅ Designing compound indexes
- ✅ Implementing covered queries
- ✅ Monitoring production database performance
- ✅ Using profiler to find bottlenecks
- ✅ Tuning aggregation pipeline execution
- ✅ Configuring connection pooling
- ✅ Scaling query performance for large data
- ✅ Setting up performance alerts
- ✅ Diagnosing timeout and resource issues

## Real-World Scenarios

### Scenario 1: High-Traffic User Search
Optimize user search with filtering, sorting, and pagination:
```javascript
// Query: Find active users by email prefix, sorted by creation
db.users.find({
  status: "active",
  email: { $regex: "^john" }
}).sort({ createdAt: -1 }).limit(50)

// Optimal index (ESR rule)
db.users.createIndex({ status: 1, createdAt: -1, email: 1 })

// Verify with explain
const stats = await users.find(...).explain("executionStats")
console.log(`Efficiency: ${stats.nReturned}/${stats.totalDocsExamined}`)
```

### Scenario 2: Analytics Query Optimization
Optimize complex aggregation for reporting:
```javascript
// Query: Monthly revenue by product with filters
db.orders.aggregate([
  { $match: { status: "completed", date: { $gte: ISODate("2024-01-01") } } },
  { $group: {
      _id: { month: { $dateToString: { format: "%Y-%m", date: "$date" } }, productId: "$productId" },
      revenue: { $sum: "$amount" }
    }
  },
  { $sort: { "_id.month": -1, revenue: -1 } }
])

// Index to support $match filtering
db.orders.createIndex({ status: 1, date: 1 })
db.orders.createIndex({ productId: 1, date: 1 })
```

### Scenario 3: Profiler-Based Optimization
Use profiler to identify slow queries:
```javascript
// Enable profiler for slow queries (>100ms)
db.setProfilingLevel(1, { slowms: 100 })

// Query slow operations
const slowOps = db.system.profile.find({
  millis: { $gt: 100 }
}).sort({ ts: -1 }).limit(10).toArray()

slowOps.forEach(op => {
  console.log(`Slow query: ${op.op} - ${op.millis}ms`)
  if (op.planSummary) console.log(`Plan: ${op.planSummary}`)
})

// Create indexes based on findings
```

### Scenario 4: Connection Pool & Batch Optimization
Optimize for high-concurrency scenarios:
```javascript
// Optimal connection pool configuration
const client = new MongoClient(uri, {
  maxPoolSize: 100,
  minPoolSize: 10,
  maxIdleTimeMS: 45000,
  serverSelectionTimeoutMS: 5000
})

// Batch operations instead of individual
const operations = users.map(user => ({
  insertOne: { document: user }
}))
await collection.bulkWrite(operations)  // Much faster than insertMany
```

## Common Pitfalls & Solutions

### ❌ Pitfall 1: Creating Too Many Indexes
```javascript
// ❌ Wrong: Index on every field
db.users.createIndex({ a: 1 })
db.users.createIndex({ b: 1 })
db.users.createIndex({ c: 1 })
db.users.createIndex({ d: 1 })
// Each index uses memory and slows writes!

// ✅ Correct: Create only needed indexes
db.users.createIndex({ email: 1 })  // Frequent filter
db.users.createIndex({ createdAt: -1 })  // Common sort
```

### ❌ Pitfall 2: Ignoring Index Selectivity
```javascript
// ❌ Wrong: Index on low-cardinality field
db.users.createIndex({ gender: 1 })  // Only 2-3 values, poor selectivity

// ✅ Correct: Index on high-cardinality fields
db.users.createIndex({ email: 1 })  // Unique, high selectivity
```

### ❌ Pitfall 3: Wrong Field Order in Compound Indexes
```javascript
// ❌ Wrong: Range field before sort
db.orders.createIndex({ amount: 1, createdAt: -1 })  // Bad order!

// ✅ Correct: ESR rule - Equality, Sort, Range
db.orders.createIndex({ status: 1, createdAt: -1, amount: 1 })
```

### ❌ Pitfall 4: Covered Query Mistake with _id
```javascript
// ❌ Wrong: Forgot _id exclusion
db.users.find({}, { projection: { email: 1, name: 1 } })  // _id included, not covered!

// ✅ Correct: Explicitly exclude _id
db.users.find({}, { projection: { email: 1, name: 1, _id: 0 } })  // Covered!
```

### ❌ Pitfall 5: Not Monitoring Index Usage
```javascript
// ❌ Wrong: Create indexes and forget about them
db.users.createIndex({ oldField: 1 })  // Never used but still slowing writes

// ✅ Correct: Monitor and remove unused indexes
const stats = await users.aggregate([{ $indexStats: {} }]).toArray()
stats.forEach(idx => {
  if (idx.accesses.ops === 0) {
    console.log(`Unused index: ${idx.name}`)
    // Can drop if safe
  }
})
```

## Tips & Best Practices

### Index Creation
- **Create early** - Before data grows and queries slow
- **ESR rule** - Equality, Sort, Range field order
- **Compound first** - Design compound indexes before singles
- **Monitor size** - Large indexes consume memory
- **Background builds** - Use background: true for large collections

### Query Optimization
- **Use explain()** - Always analyze query execution
- **IXSCAN good** - Column scan means index is used
- **COLLSCAN bad** - Collection scan means no index or poor selectivity
- **Match early** - In aggregation, $match first
- **Limit early** - Reduce documents before expensive operations

### Profiling & Monitoring
- **Profile strategically** - Enable for specific thresholds
- **Monitor metrics** - Track latency, throughput, CPU, memory
- **Alert on anomalies** - Setup thresholds for slow queries
- **Baseline performance** - Know your baseline before optimizing
- **Test changes** - Measure impact of index changes

## Related Skills to Master

- **mongodb-index-creation** - Detailed index types and creation
- **mongodb-find-queries** - Query writing and operators
- **mongodb-aggregation-pipeline** - Pipeline stage optimization
- **mongodb-schema-design** - Design for query patterns
- **mongodb-replication-sharding** - Distributed performance

## Frequently Asked Questions

**Q: When should I create an index?**
A: Create indexes on fields that are frequently filtered, sorted, or used in joins. Analyze explain() output for COLLSCAN patterns.

**Q: What's the difference between IXSCAN and COLLSCAN?**
A: IXSCAN uses an index (fast), COLLSCAN scans all documents (slow). Always aim for IXSCAN in production queries.

**Q: How do I know if an index is helping?**
A: Use explain() to check totalDocsExamined vs. nReturned ratio. Should be close (ideally equal for covered queries).

**Q: Can I force MongoDB to use a specific index?**
A: Yes, use .hint({ field: 1 }) on queries, but this should rarely be necessary if indexes are designed well.

**Q: How many indexes should a collection have?**
A: Typically 5-10 is healthy. Too many slow down writes. Remove unused indexes monthly.

**Q: What's a covered query?**
A: A query where all fields needed (including projection) exist in the index, so MongoDB never fetches documents.

**Q: How do I monitor slow queries?**
A: Use profiler (setProfilingLevel), log slow queries, and analyze system.profile collection regularly.

## Next Steps

1. **Analyze Current Queries** - Use explain() on critical queries
2. **Create Strategic Indexes** - Start with high-frequency queries
3. **Monitor Baseline** - Measure performance before optimization
4. **Test & Measure** - Verify index impact with explain
5. **Setup Profiler** - Monitor slow queries in production
6. **Iterate & Optimize** - Continuously refine index strategy

---

**Ready to optimize MongoDB for lightning-fast queries!** ⚡
