---
name: deployment-guide
description: guide - MongoDB Deployment Strategies
allowed-tools: Read
---

# /deployment-guide - MongoDB Deployment Strategies

Get guidance on deploying MongoDB for different scenarios and requirements.

## Description

Interactive deployment planning based on your use case, scale, and requirements.

## Usage

```
/deployment-guide [scenario]
```

## Deployment Scenarios

### Development Environment
```
/deployment-guide development

Your Setup:
├── Local MongoDB Instance
├── MongoDB Atlas Free Tier
└── Docker Compose Stack

Recommended:
✅ MongoDB Atlas M0 (Free)
✅ Local mongod for testing
✅ Docker for consistency

Setup Steps:
1. Create MongoDB Atlas account
2. Create free cluster (AWS, GCP, or Azure)
3. Whitelist your IP
4. Get connection string
5. Connect from application

Time to Production: < 30 minutes
```

### Single Server Deployment
```
/deployment-guide single-server

Your Setup:
├── Single MongoDB instance
├── Backup strategy
└── Basic monitoring

Recommended:
✅ MongoDB 6.0+ (enterprise grade)
✅ SSD storage (essential!)
✅ Data directory: /var/lib/mongodb
✅ Log directory: /var/log/mongodb

Configuration:
- maxPoolSize: 100
- oplogMinRetentionHours: 24
- slowms: 100 (log slow queries)

Backup Strategy:
- Daily incremental backups
- Weekly full backups
- Test restore monthly

Limitations:
❌ No high availability
❌ Single point of failure
❌ Limited scalability

Use Case: Small apps, testing, non-critical data
```

### High Availability (Replica Set)
```
/deployment-guide high-availability

Your Setup:
├── Replica Set (3+ members)
├── Automatic failover
├── Read preference options
└── Backup from secondary

Architecture:
┌─────────────────┐
│ Primary         │ ← Reads & Writes
├─────────────────┤
│ Secondary-1     │ ← Reads only
├─────────────────┤
│ Secondary-2     │ ← Reads only
├─────────────────┤
│ Arbiter (opt.)  │ ← Voting only
└─────────────────┘

Configuration:
```yaml
replication:
  replSetName: rs0
storage:
  journal:
    enabled: true
```

Benefits:
✅ Automatic failover (< 30 seconds)
✅ Data redundancy
✅ Read scaling with secondary reads
✅ Zero downtime deployments
✅ Point-in-time recovery via oplog

Maintenance:
- Monitor replication lag
- Check oplog size (at least 24 hours)
- Regular backups
- Test failover quarterly

Use Case: Production systems, critical data
```

### Sharded Cluster
```
/deployment-guide sharded

Your Setup:
├── Config Servers (3)
├── Mongos Routers (2+)
├── Shard Servers (3+)
└── Load balancer

Architecture:
┌─────────────────────────────┐
│ Application Load Balancer   │
├──────────┬──────────┬───────┤
│ Mongos-1 │ Mongos-2 │Mongos-│
├──────────┼──────────┼───────┤
│ Config   │ Config   │Config │
│ Servers  │ Servers  │Server │
├──────────┼──────────┼───────┤
│ Shard-1  │ Shard-2  │Shard- │
│ (RS)     │ (RS)     │(RS)   │
└──────────┴──────────┴───────┘

Shard Key Design:
🔑 Critical decision!
- Cardinality: High (many unique values)
- Distribution: Evenly distributed writes
- Query patterns: Frequently queried together

Examples:
✅ userId (high cardinality, good distribution)
✅ email (unique, high cardinality)
❌ status (low cardinality - creates hotspots)
❌ boolean fields (terrible cardinality)

Setup:
1. Deploy config servers (replica set)
2. Deploy mongos routers
3. Deploy shard servers (replica sets)
4. Enable sharding on database
5. Shard collections with proper key

Use Case: TB+ of data, millions of operations/sec
```

### MongoDB Atlas (Cloud)
```
/deployment-guide atlas

Recommended for Most Users!

Tiers:
M0 (Free)
  - 512MB storage
  - Shared resources
  - Development only

M2/M5 (Shared)
  - 2GB-10GB
  - Shared instances
  - Testing/staging

M10+ (Dedicated)
  - Dedicated servers
  - Production-grade
  - Replica sets, sharding

Setup Steps:
1. Create Atlas account
2. Create organization
3. Create project
4. Create cluster
5. Configure backup
6. Set up network access
7. Get connection string

Features Included:
✅ Automatic backups
✅ High availability
✅ Encryption at rest
✅ Monitoring & alerting
✅ Automated patches
✅ 99.99% SLA

Pricing: Pay as you go
```

### Multi-Region Deployment
```
/deployment-guide multi-region

Your Setup:
├── Primary Region (US-East)
├── Secondary Region (EU-West)
└── Read-only Region (Asia-Pacific)

Architecture:
┌──────────────────┐
│ US-East (Primary)│
│ - Replica Set    │
│ - Writes here    │
└────────┬─────────┘
         │
    Replication
         │
    ┌────┴─────────────┐
    │                  │
┌───▼────────┐  ┌─────▼───────┐
│EU-West(RO) │  │APAC (RO)    │
│- Reads     │  │- Reads      │
└────────────┘  └─────────────┘
```

Benefits:
✅ Data locality (faster reads)
✅ Disaster recovery
✅ High availability across regions
✅ Regulatory compliance (GDPR)

Considerations:
⚠️ Replication lag (100-300ms typical)
⚠️ Higher costs
⚠️ Complexity in operations
⚠️ Eventual consistency

Setup:
1. Deploy primary replica set
2. Add secondary members in other regions
3. Configure firewall for replication
4. Monitor replication lag
5. Set read preferences per region
```

## Pre-Deployment Checklist

```
Infrastructure
☐ SSD storage allocated
☐ Network configured
☐ Firewall rules set
☐ Backup storage prepared
☐ Monitoring tools installed

MongoDB Configuration
☐ Authentication enabled
☐ TLS/SSL configured
☐ Encryption at rest enabled
☐ Audit logging enabled
☐ Slow query logging configured

Testing
☐ Connection tested
☐ CRUD operations verified
☐ Failover tested (if replica set)
☐ Backup/restore tested
☐ Performance baseline established

Documentation
☐ Connection strings documented
☐ Backup procedures documented
☐ Disaster recovery plan
☐ Runbooks created
☐ Contact list updated
```

## Scaling Decision Tree

```
How much data?
├─ < 1GB: Single server or Atlas M0
├─ 1-10GB: Single server with backups
├─ 10-100GB: Replica set
├─ 100GB-1TB: Replica set + optimization
├─ 1-10TB: Sharded cluster (2-3 shards)
└─ > 10TB: Sharded cluster (4+ shards)

How many operations/sec?
├─ < 1K: Single server
├─ 1K-10K: Replica set
├─ 10K-100K: Replica set + indexes
├─ 100K-1M: Sharded cluster
└─ > 1M: Large sharded cluster + optimization
```

## Production Readiness Checklist

✅ High availability strategy (replica sets)
✅ Backup and recovery procedures
✅ Security (auth, encryption, audit)
✅ Monitoring and alerting
✅ Performance baseline and SLAs
✅ Disaster recovery plan
✅ Runbooks for common operations
✅ 24/7 operations support
✅ Regular testing of backups
✅ Capacity planning and growth strategy
