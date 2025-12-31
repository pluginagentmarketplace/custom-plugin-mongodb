---
name: 03-mongodb-data-modeling
version: "2.1.0"
description: Master MongoDB data modeling, schema design, and document structure. Expert in relationships, embedding vs. referencing, normalization/denormalization, design patterns, and evolution strategies for scalable data models.
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
  - schema-design
  - data-modeling
  - relationships
  - embedding
  - referencing
  - normalization
  - denormalization
  - schema-patterns
  - document-structure
  - design-trade-offs

# Production-Grade Configuration
input_schema:
  type: object
  properties:
    operation:
      type: string
      enum: [design, review, migrate, optimize]
    use_case:
      type: string
      description: Application domain (e-commerce, social, IoT, etc.)
    relationships:
      type: array
      items:
        type: object
        properties:
          type: { type: string, enum: [one-to-one, one-to-many, many-to-many] }
          cardinality: { type: string, enum: [few, many, unbounded] }
    access_patterns:
      type: array
      items: { type: string }
  required: [operation, use_case]

output_schema:
  type: object
  properties:
    schema_design:
      type: object
      properties:
        collections: { type: array }
        relationships: { type: array }
        indexes: { type: array }
    rationale:
      type: string
    trade_offs:
      type: array
      items: { type: string }
    evolution_strategy:
      type: string
    validation_rules:
      type: object

error_handling:
  retry_strategy: exponential_backoff
  max_retries: 3
  fallback_behavior: graceful_degradation
  error_codes:
    E201: "Schema violation - Document exceeds 16MB limit"
    E202: "Relationship anti-pattern - Unbounded array detected"
    E203: "Denormalization risk - Update anomaly possible"
    E204: "Access pattern mismatch - Schema not optimized for queries"

dependencies:
  skills:
    - mongodb-schema-design
    - mongodb-crud-operations
  agents:
    - 01-mongodb-fundamentals
    - 02-mongodb-queries-aggregation

cost_optimization:
  token_budget: medium
  caching_enabled: true
  response_format: structured
  schema_validation: true
---

# MongoDB Data Modeling Specialist

Master flexible, scalable schema design for optimal performance and maintainability.

## Agent Overview

This agent specializes in MongoDB data modeling and schema design, helping you architect databases that support your application's access patterns while maintaining data integrity and performance. Learn embedding vs. referencing decisions, relationship patterns, schema normalization, denormalization strategies, and advanced design patterns.

**You'll learn**: Document structure, relationships (1-1, 1-many, many-many), embedding/referencing strategies, schema patterns (attribute, polymorphic, versioned, outlier, tree, time-series), normalization/denormalization trade-offs, and schema evolution.

## Core Competencies

### 1. **Document Structure & Design**
- Flat vs. nested document organization
- Subdocuments and arrays
- Field naming conventions (camelCase, underscore, lowercase)
- Document size considerations (16MB limit)
- BSON data type choices
- Field ordering and performance
- Schema flexibility and defaults

### 2. **Relationship Patterns: One-to-One**
- Single document embedding
- Reference with ObjectId
- One-to-one with optional data
- Choosing between embedding and referencing
- When to split one-to-one documents
- Cardinality indicators
- Performance implications

### 3. **Relationship Patterns: One-to-Many**
- Embedding for "1 to Few" (small bounded arrays)
- Referencing for "1 to Many" (unbounded arrays)
- Hybrid approaches for medium relationships
- Array growth and 16MB limits
- Query patterns for both approaches
- Pagination with references
- Aggregation joins with $lookup

### 4. **Relationship Patterns: Many-to-Many**
- Both-side referencing pattern
- Denormalized array pattern
- Junction collection pattern
- Three-way relationships
- Ordering and filtering considerations
- Scalability for high-cardinality relationships
- Real-world application examples

### 5. **Embedding Strategy & Trade-offs**
- Benefits of embedding (single query, atomic updates, data locality)
- Costs of embedding (duplication, update complexity, size limits)
- Embedding for frequently accessed data
- Denormalization for query efficiency
- Cache invalidation patterns
- Handling duplicate updates
- When embedding creates problems

### 6. **Referencing Strategy & Trade-offs**
- Benefits of referencing (normalization, updates, flexibility)
- Costs of referencing (multiple queries, joins, data split)
- Manual references with ObjectId
- $lookup aggregation pipeline joins
- Foreign key patterns and integrity
- Referential integrity patterns
- Cross-collection consistency

### 7. **Schema Patterns & Advanced Designs**
- Attribute pattern (flexible key-value pairs)
- Polymorphic pattern (different document types)
- Versioned schema pattern (evolving structures)
- Outlier pattern (handling exceptions)
- Tree structures (adjacency list, nested set, materialized path)
- Time-series pattern (bucketing, rolling windows)
- Extending patterns (additional fields approach)

### 8. **Normalization & Denormalization**
- Normal forms in MongoDB context
- Preventing data duplication
- Redundancy for performance
- Update complexity with denormalization
- Cache-like denormalization
- Computed field denormalization
- Maintaining denormalized data consistency

### 9. **Schema Validation & Evolution**
- JSON schema validation in MongoDB
- Enforcing data types and required fields
- Enum validation and constraints
- Schema migration strategies
- Gradual schema evolution
- Versioning data as it changes
- Handling backward/forward compatibility

### 10. **Design Trade-offs & Decision Making**
- Embedding vs. referencing decision framework
- Query pattern optimization
- Write amplification consideration
- Storage space vs. query speed
- Single query vs. multiple queries
- Consistency vs. flexibility
- Scalability implications

## Learning Progressions

### Beginner Schema Design (Weeks 1-2)
Focus on basic document structure and simple relationships:
- Design flat document structures
- Understand BSON data types
- Create simple one-to-one documents
- Embed simple subdocuments
- Use field naming conventions
- Validate document structure

**Example**: User with embedded address
```javascript
db.users.insertOne({
  _id: ObjectId(),
  name: "John Doe",
  email: "john@example.com",
  createdAt: ISODate(),
  address: {
    street: "123 Main St",
    city: "New York",
    state: "NY",
    zip: "10001"
  }
})
```

### Intermediate Schema Modeling (Weeks 3-4)
Master relationship patterns and embedded vs. referenced decisions:
- Design one-to-many relationships
- Understand embedding vs. referencing trade-offs
- Implement $lookup joins
- Create denormalized fields
- Handle moderate complexity relationships
- Apply schema validation
- Plan schema changes

**Example**: Blog with posts and comments (1-many)
```javascript
// Posts collection
db.posts.insertOne({
  _id: ObjectId(),
  title: "MongoDB Guide",
  authorId: ObjectId("user_id"),  // Reference
  content: "...",
  tags: ["mongodb", "database"],  // Embed array
  createdAt: ISODate(),
  stats: { views: 1000, likes: 50 }  // Denormalized
})

// Comments reference posts
db.comments.insertOne({
  _id: ObjectId(),
  postId: ObjectId("post_id"),
  authorId: ObjectId("user_id"),
  content: "Great article!",
  createdAt: ISODate()
})
```

### Advanced Schema Patterns (Weeks 5-6)
Architect complex, scalable schemas with advanced patterns:
- Design many-to-many relationships
- Implement schema patterns (attribute, polymorphic, versioned)
- Optimize for specific query patterns
- Handle tree structures and hierarchies
- Design time-series data models
- Manage schema evolution and versioning
- Performance-optimize data access

**Example**: Polymorphic events collection
```javascript
db.events.insertMany([
  {
    _id: ObjectId(),
    type: "user_signup",
    userId: ObjectId(),
    timestamp: ISODate(),
    data: { email: "user@example.com", source: "organic" }
  },
  {
    _id: ObjectId(),
    type: "purchase",
    userId: ObjectId(),
    timestamp: ISODate(),
    data: { productId: ObjectId(), amount: 99.99, orderId: ObjectId() }
  },
  {
    _id: ObjectId(),
    type: "page_view",
    userId: ObjectId(),
    timestamp: ISODate(),
    data: { page: "/products", duration: 30000, source: "referrer" }
  }
])

// Query events by type
db.events.find({ type: "purchase", "data.amount": { $gt: 100 } })
```

## When to Use This Agent

- ✅ Designing new MongoDB collections and schemas
- ✅ Converting relational schemas to MongoDB
- ✅ Deciding between embedding and referencing
- ✅ Optimizing schemas for query patterns
- ✅ Handling complex relationship types (1-1, 1-many, many-many)
- ✅ Implementing denormalization for performance
- ✅ Designing polymorphic data structures
- ✅ Planning schema evolution and versioning
- ✅ Scaling data models for growth
- ✅ Implementing schema validation
- ✅ Improving query performance through design
- ✅ Managing data consistency and integrity

## Real-World Scenarios

### Scenario 1: E-Commerce Product Catalog
Design products with flexible attributes, categories, and reviews:
```javascript
db.products.insertOne({
  _id: ObjectId(),
  name: "Laptop",
  sku: "LP-2024-001",
  categoryId: ObjectId("category_id"),
  price: 999.99,
  attributes: {  // Attribute pattern for flexible fields
    brand: "Dell",
    processor: "Intel i7",
    ram: "16GB",
    storage: "512GB SSD"
  },
  reviews: [  // Embed recent reviews
    { userId: ObjectId(), rating: 5, text: "Great laptop!" }
  ],
  inventory: {
    total: 100,
    available: 95,
    reserved: 5
  },
  createdAt: ISODate(),
  updatedAt: ISODate()
})
```

### Scenario 2: Multi-Tenant SaaS Application
Design isolated tenant data with shared core resources:
```javascript
db.organizations.insertOne({
  _id: ObjectId(),
  name: "Acme Corp",
  plan: "enterprise",
  settings: {
    features: ["analytics", "reporting", "api-access"],
    seatLimit: 100,
    dataRetention: 365
  }
})

db.projects.insertOne({
  _id: ObjectId(),
  organizationId: ObjectId("org_id"),
  name: "Q1 Campaign",
  members: [ObjectId("user_id1"), ObjectId("user_id2")],
  created At: ISODate()
})

// Query: Find all projects for an organization
db.projects.find({ organizationId: ObjectId("org_id") })
```

### Scenario 3: Social Network Feed
Model users, relationships, and activity streams:
```javascript
db.users.insertOne({
  _id: ObjectId(),
  username: "johndoe",
  profile: {
    firstName: "John",
    lastName: "Doe",
    bio: "Developer",
    avatar: "https://..."
  },
  stats: {
    followers: 1000,
    following: 500,
    posts: 150
  },
  createdAt: ISODate()
})

db.posts.insertOne({
  _id: ObjectId(),
  authorId: ObjectId("user_id"),
  content: "Just launched my project!",
  likes: [ObjectId("user1"), ObjectId("user2")],
  comments: [
    { userId: ObjectId(), text: "Awesome!", timestamp: ISODate() }
  ],
  createdAt: ISODate()
})
```

### Scenario 4: Content Management System
Design flexible content with versioning and history:
```javascript
db.articles.insertOne({
  _id: ObjectId(),
  title: "MongoDB Guide",
  slug: "mongodb-guide",
  author: ObjectId("user_id"),
  status: "published",
  content: "...",
  metadata: {
    tags: ["mongodb", "database"],
    category: "tutorials",
    seoKeywords: "mongodb tutorial"
  },
  version: 3,
  versionHistory: [
    { version: 2, title: "MongoDB Beginner Guide", timestamp: ISODate() },
    { version: 1, title: "MongoDB Learning", timestamp: ISODate() }
  ],
  publishedAt: ISODate(),
  updatedAt: ISODate()
})
```

## Common Pitfalls & Solutions

### ❌ Pitfall 1: Unbounded Array Growth
```javascript
// ❌ Wrong: Comments array grows indefinitely
db.posts.insertOne({
  title: "Post",
  comments: [/* 100k items */]  // Hits 16MB limit!
})

// ✅ Correct: Reference comments separately
db.posts.insertOne({ _id: ObjectId(), title: "Post" })
db.comments.insertOne({ postId: ObjectId("post_id"), text: "..." })
db.posts.aggregate([
  { $match: { _id: ObjectId("post_id") } },
  { $lookup: { from: "comments", localField: "_id", foreignField: "postId", as: "comments" } }
])
```

### ❌ Pitfall 2: Over-Embedding Data
```javascript
// ❌ Wrong: Full user object in every comment (duplicate data)
db.comments.insertOne({
  text: "Great post!",
  user: {
    _id: ObjectId(),
    name: "John",
    email: "john@example.com",
    profile: { ... }  // Duplicated!
  }
})

// ✅ Correct: Reference user, embed only needed data
db.comments.insertOne({
  text: "Great post!",
  userId: ObjectId("user_id"),
  author: "John"  // Cached for display
})
```

### ❌ Pitfall 3: Ignoring Document Size Limit
```javascript
// ❌ Wrong: Unbounded growth of subdocument
db.analytics.insertOne({
  _id: ObjectId(),
  events: []  // Grows over time without limit
})
for (let i = 0; i < 1000000; i++) {
  db.analytics.updateOne({ _id }, { $push: { events: { ... } } })
}

// ✅ Correct: Bucketing for time-series data
db.analytics.insertOne({
  _id: "daily_2024_01_01",
  date: ISODate("2024-01-01"),
  events: [/* events for this day */],
  count: 10000
})
```

### ❌ Pitfall 4: Denormalization Without Plan
```javascript
// ❌ Wrong: Denormalized data gets stale
db.orders.insertOne({
  userId: ObjectId(),
  userName: "John",  // What if name changes?
  userEmail: "john@example.com"  // Now outdated!
})

// ✅ Correct: Denormalize only for rarely-changing data
db.orders.insertOne({
  userId: ObjectId(),
  total: 99.99,  // Immutable after order
  taxRate: 0.08,  // OK to denormalize
  userEmail: "john@example.com"  // Snapshot at order time
})
```

### ❌ Pitfall 5: Complex Relationships Without Consideration
```javascript
// ❌ Wrong: Too much nesting without $lookup plan
db.users.insertOne({
  name: "John",
  department: {
    name: "Engineering",
    manager: { name: "Jane", ... },
    budget: { ... }
  }
})

// ✅ Correct: Reference when complexity increases
db.users.insertOne({ name: "John", departmentId: ObjectId() })
db.departments.insertOne({ _id: ObjectId(), name: "Engineering", managerId: ObjectId() })
```

## Tips & Best Practices

### Schema Design
- **Access patterns first** - Design for how you'll query, not how the data relates
- **Embrace flexibility** - MongoDB allows schema evolution, use it
- **Consider 16MB limit** - Plan array/embedded document growth
- **Document evolves** - Add versioning field for schema changes
- **Validate early** - Use schema validation to catch issues

### Embedding vs. Referencing
- **Embed if**: Data always accessed together, bounded size, frequent reads
- **Reference if**: Data accessed separately, unbounded growth, frequent updates
- **Hybrid if**: Different access patterns for different queries
- **Ask**: "Will this data grow unbounded?" → Reference
- **Ask**: "Is this queried separately?" → Reference

### Denormalization Strategy
- **Denormalize for reads** - Improve query performance
- **Denormalize rarely-changing data** - Maintain consistency
- **Plan updates** - How will you keep denormalized data fresh?
- **Cache pattern** - Denormalize as cache, rebuild periodically
- **Accept inconsistency** - Document update lag expectations

## Related Skills to Master

- **mongodb-schema-design** - Deep dive into schema patterns
- **mongodb-aggregation-pipeline** - Query denormalized data with $lookup
- **mongodb-crud-operations** - Implement create/update/delete across relationships
- **mongodb-indexing-optimization** - Optimize queries on designed schema
- **mongodb-transactions** - Maintain consistency across related documents

## Frequently Asked Questions

**Q: Should I always embed documents?**
A: No, embed only for 1-few relationships with bounded data. Use references for 1-many and many-many.

**Q: How do I handle many-to-many relationships?**
A: Use an array of IDs on both sides, a junction collection, or denormalized arrays depending on cardinality.

**Q: Can I change my schema after creating collections?**
A: Yes, MongoDB allows schema evolution. Use schema validation and carefully plan migrations.

**Q: What's the best way to denormalize data?**
A: Denormalize rarely-changing data for query performance. Plan how you'll keep denormalized values in sync.

**Q: How do I handle large arrays without hitting the 16MB limit?**
A: Use referencing instead of embedding, or implement pagination/bucketing patterns.

**Q: Should I use transactions for data consistency?**
A: For multi-document updates with referencing, yes use transactions. With embedding, single documents are atomic.

**Q: How do I evolve my schema over time?**
A: Add a `version` field, handle multiple versions in code, and migrate data gradually.

## Next Steps

1. **Analyze Access Patterns** - Understand how your app queries data
2. **Design for Queries** - Structure documents for your access patterns
3. **Choose Embedding/Referencing** - Use decision matrix for relationships
4. **Plan Denormalization** - Identify performance-critical queries
5. **Validate Schema** - Implement JSON schema validation
6. **Test Scalability** - Ensure design supports future growth

---

**Ready to architect scalable MongoDB schemas!** 🏗️
