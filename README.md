<div align="center">

# MongoDB Developer Plugin

### Complete MongoDB Mastery for Claude Code

**Master MongoDB from fundamentals to production with 7 specialized agents and 12 production-ready skills**

[![Verified](https://img.shields.io/badge/Verified-Working-success?style=flat-square&logo=checkmarx)](https://github.com/pluginagentmarketplace/custom-plugin-mongodb)
[![License](https://img.shields.io/badge/License-MIT-yellow?style=flat-square)](LICENSE)
[![Version](https://img.shields.io/badge/Version-2.0.0-blue?style=flat-square)](https://github.com/pluginagentmarketplace/custom-plugin-mongodb)
[![Status](https://img.shields.io/badge/Status-Production_Ready-brightgreen?style=flat-square)](https://github.com/pluginagentmarketplace/custom-plugin-mongodb)
[![Agents](https://img.shields.io/badge/Agents-7-orange?style=flat-square)](#agents-overview)
[![Skills](https://img.shields.io/badge/Skills-12-purple?style=flat-square)](#skills-reference)
[![SASMP](https://img.shields.io/badge/SASMP-v1.3.0-blueviolet?style=flat-square)](#)

[![MongoDB](https://img.shields.io/badge/MongoDB-47A248?style=for-the-badge&logo=mongodb&logoColor=white)](skills/mongodb-crud/)
[![Atlas](https://img.shields.io/badge/Atlas-00684A?style=for-the-badge&logo=mongodb&logoColor=white)](skills/mongodb-atlas-setup/)
[![NoSQL](https://img.shields.io/badge/NoSQL-4DB33D?style=for-the-badge&logo=mongodb&logoColor=white)](skills/mongodb-schema-design/)

[Quick Start](#quick-start) | [Agents](#agents-overview) | [Skills](#skills-reference) | [Commands](#commands)

</div>

---

## Verified Installation

> **This plugin has been tested and verified working on Claude Code.**
> Last verified: December 2025

---

## Quick Start

### Option 1: Install from GitHub (Recommended)

```bash
# Step 1: Add the marketplace from GitHub
/plugin add marketplace pluginagentmarketplace/custom-plugin-mongodb

# Step 2: Install the plugin
/plugin install mongodb-developer-plugin@pluginagentmarketplace-mongodb

# Step 3: Restart Claude Code to load new plugins
```

### Option 2: Clone and Load Locally

```bash
# Clone the repository
git clone https://github.com/pluginagentmarketplace/custom-plugin-mongodb.git

# Navigate to the directory in Claude Code
cd custom-plugin-mongodb

# Load the plugin
/plugin load .
```

After loading, restart Claude Code.

### Verify Installation

After restarting Claude Code, verify the plugin is loaded. You should see these agents available:

```
custom-plugin-mongodb:01-mongodb-fundamentals
custom-plugin-mongodb:02-mongodb-queries-aggregation
custom-plugin-mongodb:03-mongodb-data-modeling
custom-plugin-mongodb:04-mongodb-performance-indexing
custom-plugin-mongodb:05-mongodb-replication-sharding
custom-plugin-mongodb:06-mongodb-security-administration
custom-plugin-mongodb:07-mongodb-application-development
```

---

## Available Skills

Once installed, these 12 skills become available:

| Skill | Invoke Command | Description |
|-------|----------------|-------------|
| Atlas Setup | `Skill("mongodb-developer-plugin:mongodb-atlas-setup")` | Cloud setup, cluster configuration |
| CRUD Operations | `Skill("mongodb-developer-plugin:mongodb-crud")` | Create, Read, Update, Delete |
| Find Queries | `Skill("mongodb-developer-plugin:mongodb-find-queries")` | Queries, filters, operators |
| Aggregation | `Skill("mongodb-developer-plugin:mongodb-aggregation")` | Pipeline, data transformation |
| Schema Design | `Skill("mongodb-developer-plugin:mongodb-schema-design")` | Data modeling, patterns |
| Index Creation | `Skill("mongodb-developer-plugin:mongodb-index-creation")` | Index types, strategies |
| Indexing | `Skill("mongodb-developer-plugin:mongodb-indexing")` | Query optimization |
| Transactions | `Skill("mongodb-developer-plugin:mongodb-transactions")` | ACID operations |
| Replication | `Skill("mongodb-developer-plugin:mongodb-replication")` | Replica sets, sharding |
| Authentication | `Skill("mongodb-developer-plugin:mongodb-authentication")` | User security, SCRAM |
| Security | `Skill("mongodb-developer-plugin:mongodb-security")` | Enterprise security |
| Integration | `Skill("mongodb-developer-plugin:mongodb-integration")` | Application patterns |

---

## What This Plugin Does

This plugin provides **7 specialized agents** and **12 production-ready skills** for complete MongoDB mastery:

| Agent | Purpose |
|-------|---------|
| **Fundamentals** | Document model, CRUD, Atlas setup |
| **Query & Aggregation** | Find operations, pipelines, analytics |
| **Data Modeling** | Schema design, relationships, patterns |
| **Performance** | Indexes, optimization, query tuning |
| **Replication** | High availability, sharding, scaling |
| **Security** | Authentication, encryption, backups |
| **Development** | Drivers, transactions, production patterns |

---

## Agents Overview

### 7 Implementation Agents

Each agent is designed to **do the work**, not just explain:

| Agent | Capabilities | Example Prompts |
|-------|--------------|-----------------|
| **Fundamentals** | Document model, collections, basics | `"MongoDB basics"`, `"Atlas setup"` |
| **Query & Aggregation** | Queries, pipelines, filtering | `"Aggregation pipeline"`, `"Complex query"` |
| **Data Modeling** | Schema design, relationships | `"Design schema"`, `"Embed vs reference"` |
| **Performance** | Indexes, optimization | `"Create index"`, `"Query performance"` |
| **Replication** | Replica sets, sharding | `"Setup replica set"`, `"Sharding strategy"` |
| **Security** | Auth, encryption, backups | `"Setup authentication"`, `"Encrypt data"` |
| **Development** | Drivers, transactions | `"Transaction example"`, `"Driver setup"` |

---

## Commands

4 interactive commands for MongoDB workflows:

| Command | Usage | Description |
|---------|-------|-------------|
| `/learn-mongodb` | `/learn-mongodb` | Personalized learning journey |
| `/query-builder` | `/query-builder` | Build & optimize queries |
| `/deployment-guide` | `/deployment-guide` | Deployment strategies |
| `/tutorial` | `/tutorial` | Step-by-step projects |

---

## Skills Reference

Each skill includes **Golden Format** content:
- `assets/` - Configuration templates and setup files
- `scripts/` - Automation and validation scripts
- `references/` - Methodology guides and best practices

### All 12 Skills by Category

| Category | Skills |
|----------|--------|
| **Setup** | mongodb-atlas-setup |
| **Operations** | mongodb-crud, mongodb-find-queries |
| **Analytics** | mongodb-aggregation |
| **Design** | mongodb-schema-design |
| **Performance** | mongodb-index-creation, mongodb-indexing |
| **Transactions** | mongodb-transactions |
| **Scaling** | mongodb-replication |
| **Security** | mongodb-authentication, mongodb-security |
| **Integration** | mongodb-integration |

---

## Usage Examples

### Example 1: Create Aggregation Pipeline

```javascript
// Before: Manual data processing

// After (with Query & Aggregation agent):
Skill("mongodb-developer-plugin:mongodb-aggregation")

// Generates:
// - Stage-by-stage pipeline
// - $match, $group, $project operators
// - Performance optimization
// - Index recommendations
```

### Example 2: Design Schema

```javascript
// Before: No schema planning

// After (with Data Modeling agent):
Skill("mongodb-developer-plugin:mongodb-schema-design")

// Provides:
// - Embedding vs referencing decisions
// - Denormalization patterns
// - Index design
// - Query optimization
```

### Example 3: Setup Replica Set

```javascript
// Before: Single server setup

// After (with Replication agent):
Skill("mongodb-developer-plugin:mongodb-replication")

// Creates:
// - 3-node replica set
// - Automatic failover
// - Read preference configuration
// - Monitoring setup
```

---

## Plugin Structure

```
custom-plugin-mongodb/
├── .claude-plugin/
│   ├── plugin.json           # Plugin manifest
│   └── marketplace.json      # Marketplace config
├── agents/                   # 7 specialized agents
│   ├── 01-mongodb-fundamentals.md
│   ├── 02-mongodb-queries-aggregation.md
│   ├── 03-mongodb-data-modeling.md
│   ├── 04-mongodb-performance-indexing.md
│   ├── 05-mongodb-replication-sharding.md
│   ├── 06-mongodb-security-administration.md
│   └── 07-mongodb-application-development.md
├── skills/                   # 12 skills (Golden Format)
│   ├── mongodb-aggregation/SKILL.md
│   ├── mongodb-atlas-setup/SKILL.md
│   ├── mongodb-authentication/SKILL.md
│   ├── mongodb-crud/SKILL.md
│   ├── mongodb-find-queries/SKILL.md
│   ├── mongodb-index-creation/SKILL.md
│   ├── mongodb-indexing/SKILL.md
│   ├── mongodb-integration/SKILL.md
│   ├── mongodb-replication/SKILL.md
│   ├── mongodb-schema-design/SKILL.md
│   ├── mongodb-security/SKILL.md
│   └── mongodb-transactions/SKILL.md
├── commands/                 # 4 slash commands
│   ├── deployment-guide.md
│   ├── learn-mongodb.md
│   ├── query-builder.md
│   └── tutorial.md
├── hooks/hooks.json
├── README.md
├── CHANGELOG.md
├── QUICK-START.md
├── BEST-PRACTICES.md
└── LICENSE
```

---

## Technology Coverage

| Category | Technologies |
|----------|--------------|
| **Database** | MongoDB 6.x/7.x, Document Model |
| **Cloud** | MongoDB Atlas, AWS, GCP, Azure |
| **Drivers** | Node.js, Python, Java, Go, .NET |
| **Tools** | Compass, mongosh, mongodump |
| **Security** | SCRAM, X.509, LDAP, Encryption |
| **Scaling** | Replica Sets, Sharding, Zone Sharding |
| **Performance** | Indexes, Query Optimizer, Profiler |
| **Integration** | Change Streams, Transactions, Time Series |

---

## Learning Paths

| Path | Duration | Focus |
|------|----------|-------|
| **Beginner** | Week 1-2 | Document model, CRUD, Atlas |
| **Intermediate** | Week 3-4 | Queries, schemas, indexes |
| **Advanced** | Week 5-6 | Aggregation, optimization, scaling |
| **Expert** | Week 7+ | Distributed systems, enterprise security |

### Real-World Projects
- Blog Platform - Users, posts, comments
- E-Commerce - Products, orders, inventory
- Analytics Dashboard - Real-time aggregation
- Chat Application - Real-time messaging

---

## Requirements

| Requirement | Version |
|-------------|---------|
| MongoDB | 5.0+ |
| Node.js (for drivers) | 16+ |
| mongosh | Latest |
| MongoDB Compass | Latest |

---

## Best Practices

- **Schema Design**: Embed for read performance, reference for write performance
- **Indexing**: Create indexes for frequent queries
- **Security**: Always enable authentication in production
- **Replication**: Minimum 3 nodes for high availability
- **Monitoring**: Use Atlas or self-managed monitoring tools
- **Backups**: Regular backups with point-in-time recovery

---

## Metadata

| Field | Value |
|-------|-------|
| **Last Updated** | 2025-12-28 |
| **Maintenance Status** | Active |
| **SASMP Version** | 1.3.0 |
| **Support** | [Issues](../../issues) |

---

## License

MIT License - See [LICENSE](LICENSE) for details.

---

## Contributing

Contributions are welcome:

1. Fork the repository
2. Create a feature branch
3. Follow the Golden Format for new skills
4. Submit a pull request

---

## Contributors

**Authors:**
- **Dr. Umit Kacar** - Senior AI Researcher & Engineer
- **Muhsin Elcicek** - Senior Software Architect

---

<div align="center">

**Master MongoDB with AI assistance!**

[![Made for MongoDB](https://img.shields.io/badge/Made%20for-MongoDB%20Developers-47A248?style=for-the-badge&logo=mongodb)](https://github.com/pluginagentmarketplace/custom-plugin-mongodb)

**Built by Dr. Umit Kacar & Muhsin Elcicek**

*Comprehensive MongoDB learning from basics to production*

</div>
