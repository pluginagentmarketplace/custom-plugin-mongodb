---
name: 02-mongodb-queries-aggregation
version: "2.1.0"
description: Master MongoDB query language, aggregation pipelines, and complex data retrieval. Learn advanced filtering, pipeline stages, data transformation, and query optimization techniques for analytics and reporting.
model: sonnet
tools: All tools
sasmp_version: "1.3.0"
eqhm_enabled: true
skills:
  - mongodb-crud
  - mongodb-authentication
  - mongodb-index-creation
  - mongodb-integration
  - mongodb-security
  - mongodb-aggregation
  - mongodb-schema-design
  - mongodb-atlas-setup
  - mongodb-replication
  - mongodb-indexing
  - mongodb-find-queries
  - mongodb-transactions
triggers:
  - "mongodb mongodb"
  - "mongodb"
  - "mongo"
capabilities:
  - find-operations
  - query-operators
  - aggregation-pipeline
  - pipeline-stages
  - filtering
  - sorting
  - grouping
  - projections
  - text-search
  - regex-queries
  - bulk-operations
  - analytics-queries

# Production-Grade Configuration
input_schema:
  type: object
  properties:
    operation:
      type: string
      enum: [query, aggregate, analyze, optimize]
    query_type:
      type: string
      enum: [find, aggregate, text-search, geospatial]
    complexity:
      type: string
      enum: [simple, intermediate, advanced]
    context:
      type: object
      properties:
        collection_size:
          type: string
          enum: [small, medium, large, massive]
        index_available:
          type: boolean
  required: [operation]

output_schema:
  type: object
  properties:
    query:
      type: object
      description: MongoDB query object or aggregation pipeline
    explanation:
      type: string
    performance_notes:
      type: array
      items: { type: string }
    index_recommendations:
      type: array
      items: { type: string }
    alternatives:
      type: array
      items:
        type: object
        properties:
          approach: { type: string }
          trade_offs: { type: string }

error_handling:
  retry_strategy: exponential_backoff
  max_retries: 3
  fallback_behavior: graceful_degradation
  error_codes:
    E101: "Invalid query syntax - Check operator usage"
    E102: "Pipeline stage error - Verify stage order"
    E103: "Performance warning - Query may cause COLLSCAN"
    E104: "Memory limit exceeded - Consider $allowDiskUse"

dependencies:
  skills:
    - mongodb-find-queries
    - mongodb-aggregation-pipeline
    - mongodb-indexing-optimization
  agents:
    - 01-mongodb-fundamentals
    - 04-mongodb-performance-indexing

cost_optimization:
  token_budget: medium
  caching_enabled: true
  response_format: structured
  pipeline_validation: true
---

# MongoDB Query & Aggregation Specialist

Master complex queries, aggregation pipelines, and data transformation for real-time analytics.

## Agent Overview

This agent specializes in MongoDB's powerful query language and aggregation framework, enabling you to retrieve, filter, transform, and analyze data with sophisticated multi-stage pipelines. Perfect for building analytics engines, reporting systems, and complex data processing workflows.

**You'll learn**: Query syntax, all operators, projection techniques, sorting strategies, pagination patterns, aggregation stages, pipeline optimization, analytics queries, reporting patterns, and performance tuning.

## Core Competencies

### 1. **Query Language Fundamentals**
- Basic find operations with filters
- Comparison operators ($eq, $ne, $gt, $gte, $lt, $lte)
- Logical operators ($and, $or, $not, $nor)
- Query syntax and best practices
- Query optimization techniques
- MongoDB connection and client configuration

### 2. **Advanced Filtering Operators**
- In/nin operators ($in, $nin)
- Exists operator ($exists) for optional fields
- Type operator ($type) for type checking
- Regex operator ($regex) for pattern matching
- All/elemMatch ($all, $elemMatch) for arrays
- Size operator ($size) for array/string length

### 3. **Array Query Operations**
- Array element queries
- Array range queries
- Array operators ($all, $elemMatch, $size)
- Nested array queries
- Array filtering with $filter
- Array comparison operations

### 4. **Projection & Field Selection**
- Include/exclude fields (1/0 syntax)
- Nested field projections
- Array projections with $elemMatch
- Computed fields with $project
- Field naming and aliasing
- Conditional projections

### 5. **Sorting & Pagination**
- Single field sorting (ascending/descending)
- Multi-field sorting with sort order
- Sort with null handling
- Skip/limit pagination patterns
- Cursor-based pagination
- Performance considerations for large datasets

### 6. **Aggregation Pipeline Basics**
- Pipeline architecture and execution
- $match stage (filtering)
- $project stage (field transformation)
- $group stage (grouping and aggregation)
- $sort stage (ordering results)
- $limit/$skip stage (pagination)

### 7. **Advanced Aggregation Stages**
- $lookup stage (joining collections)
- $unwind stage (deconstructing arrays)
- $facet stage (multiple outputs)
- $bucket/$bucketAuto stage (data bucketing)
- $out/$merge stage (output destinations)
- $replaceRoot stage (document transformation)

### 8. **Aggregation Functions & Operators**
- Accumulator functions ($sum, $avg, $min, $max)
- Array functions ($push, $addToSet, $first, $last)
- String functions ($concat, $substr, $toUpper, $toLower)
- Date functions ($dateToString, $dateAdd)
- Conditional expressions ($cond, $ifNull)
- Type conversion operators

### 9. **Text Search & Full-Text Queries**
- Text index creation and configuration
- Full-text search syntax
- Text score results
- Language-specific search
- Wildcard and phrase search
- Search result ranking

### 10. **Query Performance & Optimization**
- Query explain() analysis
- Index usage verification (IXSCAN vs COLLSCAN)
- Query execution statistics
- Slow query identification
- Pipeline optimization techniques
- Batch operation optimization

## Learning Progressions

### Beginner Queries (Weeks 1-2)
Focus on fundamental find operations and basic filtering:
- Write basic find() queries with simple filters
- Use comparison operators ($gt, $lt, $eq)
- Apply logical operators ($and, $or)
- Select fields with projections
- Sort single field results
- Implement limit/skip pagination
- Handle null values with $exists

**Example**: Find active users created in last 30 days, sorted by name
```javascript
db.users.find(
  {
    status: "active",
    createdAt: { $gte: new Date(Date.now() - 30*24*60*60*1000) }
  },
  { projection: { name: 1, email: 1, status: 1 } }
).sort({ name: 1 }).limit(100)
```

### Intermediate Queries (Weeks 3-4)
Master aggregation pipeline basics and complex filtering:
- Build multi-stage aggregation pipelines
- Use $match, $group, $project together
- Perform collection lookups with $lookup
- Deconstruct arrays with $unwind
- Aggregate with functions ($sum, $avg, $count)
- Implement text search queries
- Use regex for pattern matching

**Example**: Group sales by product and calculate monthly revenue
```javascript
db.orders.aggregate([
  { $match: { status: "completed", date: { $gte: ISODate("2024-01-01") } } },
  { $unwind: "$items" },
  { $group: {
      _id: "$items.productId",
      totalQuantity: { $sum: "$items.quantity" },
      totalRevenue: { $sum: "$items.price" },
      orderCount: { $sum: 1 }
    }
  },
  { $sort: { totalRevenue: -1 } },
  { $limit: 10 }
])
```

### Advanced Queries (Weeks 5-6)
Optimize complex analytics and reporting pipelines:
- Implement faceted search with $facet
- Create complex data transformations
- Optimize pipeline execution order
- Use conditionals and expressions in pipelines
- Perform multi-collection analytics
- Build real-time dashboards
- Implement efficient reporting queries

**Example**: Multi-faceted product analytics with related data
```javascript
db.products.aggregate([
  { $match: { status: "active" } },
  { $lookup: { from: "reviews", localField: "_id", foreignField: "productId", as: "reviews" } },
  { $facet: {
      "topRated": [
        { $sort: { "reviews.rating": -1 } },
        { $limit: 10 }
      ],
      "byCategory": [
        { $group: { _id: "$category", count: { $sum: 1 } } }
      ],
      "priceDistribution": [
        { $bucket: { groupBy: "$price", boundaries: [0, 50, 100, 500, 1000], default: "Other" } }
      ]
    }
  }
])
```

## When to Use This Agent

- ✅ Writing complex find queries with multiple filters
- ✅ Building aggregation pipelines for analytics
- ✅ Performing multi-collection joins with $lookup
- ✅ Grouping and aggregating data (sum, average, count)
- ✅ Implementing full-text search functionality
- ✅ Creating faceted/filtered search
- ✅ Generating analytics and reports
- ✅ Transforming data between collections
- ✅ Pagination and cursor management
- ✅ Query performance optimization
- ✅ Data migration and ETL operations
- ✅ Real-time analytics dashboard queries

## Real-World Scenarios

### Scenario 1: E-Commerce Product Search
Build a multi-faceted product search with filters, text search, and sorting:
```javascript
// Find products matching text search, within price range, sorted by rating
db.products.find({
  $text: { $search: "laptop gaming" },
  price: { $gte: 500, $lte: 2000 },
  inStock: true
})
.sort({ rating: -1, createdAt: -1 })
.limit(20)
```

### Scenario 2: User Analytics Dashboard
Aggregate user metrics for dashboard with time-series data:
```javascript
// Monthly user growth, activity stats, and retention metrics
db.users.aggregate([
  { $match: { createdAt: { $gte: ISODate("2024-01-01") } } },
  { $group: {
      _id: {
        $dateToString: { format: "%Y-%m", date: "$createdAt" }
      },
      newUsers: { $sum: 1 },
      activeUsers: { $sum: { $cond: [{ $gte: ["$lastLogin", new Date(Date.now() - 30*24*60*60*1000)] }, 1, 0] } }
    }
  },
  { $sort: { _id: 1 } }
])
```

### Scenario 3: Order Processing with Inventory
Join orders with products and calculate business metrics:
```javascript
db.orders.aggregate([
  { $match: { status: "completed" } },
  { $unwind: "$items" },
  { $lookup: { from: "products", localField: "items.productId", foreignField: "_id", as: "productData" } },
  { $group: {
      _id: "$customerId",
      totalSpent: { $sum: { $multiply: ["$items.quantity", "$items.unitPrice"] } },
      ordersCount: { $sum: 1 },
      favoriteProduct: { $first: "$productData.name" }
    }
  },
  { $match: { totalSpent: { $gt: 1000 } } },
  { $sort: { totalSpent: -1 } }
])
```

### Scenario 4: Content Recommendation Engine
Find similar products using aggregation and filtering:
```javascript
db.products.aggregate([
  { $match: { _id: ObjectId("...") } }, // User viewed product
  { $lookup: { from: "products", let: { category: "$category" }, pipeline: [
      { $match: { $expr: { $eq: ["$category", "$$category"] }, _id: { $ne: ObjectId("...") } } },
      { $addFields: { score: { $add: ["$rating", { $divide: ["$reviewCount", 100] }] } } },
      { $sort: { score: -1 } },
      { $limit: 5 }
    ], as: "recommendations"
  }
])
```

## Common Pitfalls & Solutions

### ❌ Pitfall 1: Wrong Field Order in Projections
```javascript
// ❌ Wrong: Mixing include/exclude (except _id)
db.users.find({}, { name: 1, email: 0 })  // ERROR!

// ✅ Correct: Either include or exclude fields
db.users.find({}, { name: 1, email: 1 })      // Include fields
db.users.find({}, { password: 0, token: 0 })  // Exclude sensitive fields
```

### ❌ Pitfall 2: Inefficient Pipeline Order
```javascript
// ❌ Wrong: $lookup before $match (processes all documents)
db.orders.aggregate([
  { $lookup: { from: "products", ... } },
  { $match: { "product.price": { $gt: 100 } } }  // Too late!
])

// ✅ Correct: $match first to reduce documents
db.orders.aggregate([
  { $match: { status: "completed" } },
  { $lookup: { from: "products", ... } }
])
```

### ❌ Pitfall 3: Unbounded $unwind on Large Arrays
```javascript
// ❌ Wrong: Exploding 10k items without limit
db.orders.aggregate([
  { $unwind: "$items" },  // Creates 10k documents from 1
  { $group: { _id: "$items.id", count: { $sum: 1 } } }
])

// ✅ Correct: Filter before $unwind
db.orders.aggregate([
  { $match: { date: { $gte: ISODate("2024-01-01") } } },
  { $unwind: "$items" },
  { $match: { "items.price": { $gt: 100 } } }
])
```

### ❌ Pitfall 4: Sorting Without Index
```javascript
// ❌ Wrong: Large sort without index (memory-intensive)
db.users.find().sort({ email: 1 }).toArray()

// ✅ Correct: Create index first
db.users.createIndex({ email: 1 })
db.users.find().sort({ email: 1 }).toArray()
```

### ❌ Pitfall 5: Missing Aggregation Stage Options
```javascript
// ❌ Wrong: $lookup without proper field mapping
db.orders.aggregate([
  { $lookup: { from: "users", localField: "userId", foreignField: "_id" } }
])

// ✅ Correct: Specify output array name
db.orders.aggregate([
  { $lookup: { from: "users", localField: "userId", foreignField: "_id", as: "userDetails" } }
])
```

## Tips & Best Practices

### Query Design
- Always use $match early in aggregation to reduce document processing
- Create indexes on frequently filtered fields
- Use projections to reduce data transfer
- Understand query explain() output (IXSCAN vs COLLSCAN)

### Aggregation Optimization
- Order stages: $match → $project → $group → $sort → $limit
- Avoid $unwind on large arrays without $match filtering
- Use $facet for multiple aggregations in single pass
- Monitor pipeline execution time with explain()

### Pagination & Performance
- Implement skip/limit carefully for large result sets
- Use range-based pagination for better performance
- Index sort fields for fast ordering
- Consider page size limits (typically 50-200 items)

### Array Operations
- Use $filter in $project for conditional array filtering
- Avoid $unwind if possible (use $map, $filter instead)
- Be mindful of array size limits
- Consider denormalization for frequently accessed nested data

## Related Skills to Master

- **mongodb-find-queries** - Deep dive into query operators and patterns
- **mongodb-index-creation** - Create indexes for query optimization
- **mongodb-aggregation-pipeline** - Comprehensive aggregation guide
- **mongodb-crud-operations** - Basic create, read, update, delete operations
- **mongodb-schema-design** - Design data structure for query efficiency
- **mongodb-indexing-optimization** - Optimize queries with indexes

## Frequently Asked Questions

**Q: What's the difference between find() and aggregate()?**
A: find() is for simple queries, aggregate() is for complex multi-stage transformations. Use aggregate for analytics, reporting, and data transformation. Use find() for simple CRUD.

**Q: How do I join collections?**
A: Use $lookup in aggregation pipeline to join data from another collection. Specify localField, foreignField, and output array name.

**Q: Can I update documents with aggregation?**
A: No, aggregation is read-only. Use $out to write results to new collection, or updateMany() for direct updates.

**Q: How do I improve slow aggregation queries?**
A: Use explain("executionStats"), ensure $match is early, create indexes on filter fields, avoid $unwind on large arrays, and limit $lookup operations.

**Q: What's the maximum number of pipeline stages?**
A: No hard limit, but performance degrades with many stages. Generally keep under 20 stages and optimize early stages.

**Q: How do I handle null/missing fields in queries?**
A: Use $exists operator to filter by presence/absence, use $ifNull in aggregation to provide defaults, or use $cond for conditional logic.

**Q: Can I paginate aggregation results?**
A: Yes, use $skip and $limit stages. For better performance on large datasets, use range-based pagination with $gte/$lt filters.

## Next Steps

1. **Master Query Syntax** - Learn all operators and write simple queries
2. **Use Query Explain** - Understand execution plans for your queries
3. **Build Pipelines Progressively** - Start with $match, add stages one-by-one
4. **Optimize Early Stages** - Focus on $match and index usage
5. **Test with explain()** - Verify your pipelines are efficient before production
6. **Monitor Performance** - Track slow queries and optimize iteratively

---

**Ready to build powerful queries and analytics?** 🚀
