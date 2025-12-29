---
name: query-builder
description: builder - MongoDB Query Helper
allowed-tools: Read
---

# /query-builder - MongoDB Query Helper

Build and optimize MongoDB queries with interactive guidance.

## Description

Get help writing MongoDB queries, aggregation pipelines, and query optimization.

## Usage

```
/query-builder [query-type]
```

## Query Types

### Find Query Builder
```
/query-builder find

What do you want to find?
1. Simple equality: field = value
2. Comparison: field > value
3. Array contains value
4. Nested document
5. Multiple conditions (AND)
6. Multiple conditions (OR)

Choose [1-5]: 2

Which field and operator?
- Field: age
- Operator: greater than (>)

Generated Query:
> db.users.find({ age: { $gt: 30 } })

Want to:
1. Add more conditions
2. Add sorting
3. Add limit
4. Execute
5. Show explanation
```

### Aggregation Pipeline Builder
```
/query-builder aggregation

Build your pipeline step by step...

Step 1 - Filter documents
What's the filter condition?
- status = active

Step 2 - Transform data
What do you want to calculate?
- Count users per category
- Average age per category

Generated Pipeline:
> db.users.aggregate([
  { $match: { status: 'active' } },
  { $group: {
    _id: '$category',
    count: { $sum: 1 },
    avgAge: { $avg: '$age' }
  }}
])
```

### Index Recommendation
```
/query-builder index

What's your query?
> db.users.find({ email: 'test@example.com' })

Current Performance: COLLSCAN (full table scan)
⚠️  No index found

Recommendation:
> db.users.createIndex({ email: 1 })

Estimated Improvement: 100-1000x faster
```

### Query Optimization
```
/query-builder optimize

Paste your slow query...

Analysis:
❌ Problem 1: COLLSCAN instead of IXSCAN
❌ Problem 2: Unnecessary projection
✅ Good: Correct operators used

Suggestions:
1. Add index on 'email' field
2. Move $match before $group in pipeline
3. Use covering query to avoid fetch

Optimized Query:
> db.users.find(
  { email: 'test@example.com' },
  { email: 1, name: 1, _id: 0 }
)
```

## Common Patterns

### Pattern: User Lookup
```javascript
// Find users and their orders
db.users.aggregate([
  { $match: { status: 'active' } },
  { $lookup: {
    from: 'orders',
    localField: '_id',
    foreignField: 'userId',
    as: 'orders'
  }},
  { $unwind: '$orders' },
  { $group: {
    _id: '$_id',
    name: { $first: '$name' },
    orderCount: { $sum: 1 },
    totalSpent: { $sum: '$orders.amount' }
  }}
])
```

### Pattern: Time Series Aggregation
```javascript
// Daily sales summary
db.orders.aggregate([
  { $match: {
    createdAt: { $gte: ISODate('2024-01-01') }
  }},
  { $group: {
    _id: { $dateToString: { format: '%Y-%m-%d', date: '$createdAt' } },
    sales: { $sum: '$amount' },
    orderCount: { $sum: 1 },
    avgOrderValue: { $avg: '$amount' }
  }},
  { $sort: { '_id': 1 } }
])
```

### Pattern: Text Search
```javascript
// Full-text search with relevance
db.products.aggregate([
  { $match: {
    $text: { $search: 'mongodb database' }
  }},
  { $addFields: {
    score: { $meta: 'textScore' }
  }},
  { $sort: { score: -1 } },
  { $limit: 10 }
])
```

## Query Explanation

```
/query-builder explain

Paste your query:
> db.users.find({ email: 'test@example.com' })

Execution Plan:
┌─ executionStages
│  ├─ stage: COLLSCAN ⚠️
│  ├─ nReturned: 1 ✓
│  └─ totalDocsExamined: 1000 ❌
│     (Scanned 1000 documents to find 1!)

Performance Metrics:
- Execution Time: 45ms
- Documents Scanned: 1000
- Documents Returned: 1
- Efficiency: 0.1%

Recommendation: Create index on 'email'
```

## Interactive Examples

```
/query-builder example find-by-email
/query-builder example group-by-category
/query-builder example recent-orders
/query-builder example text-search
/query-builder example geospatial
```

## Tips & Best Practices

✅ Use $match early in aggregation pipeline
✅ Index fields used in $match
✅ Use covering queries when possible
✅ Avoid $lookup with large foreign collections
✅ Limit documents before expensive operations
✅ Use batch processing for large datasets
✅ Always explain() your queries
✅ Monitor query performance in production

## When to Use This Command

- Writing complex queries
- Debugging slow queries
- Learning query syntax
- Optimizing existing queries
- Understanding aggregation pipeline
- Planning indexes
- Understanding explain() output
