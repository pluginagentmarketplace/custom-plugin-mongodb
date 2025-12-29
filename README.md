<div align="center">

<!-- Animated Typing Banner -->
<img src="https://readme-typing-svg.demolab.com?font=Fira+Code&weight=600&size=28&duration=3000&pause=1000&color=2E9EF7&center=true&vCenter=true&multiline=true&repeat=true&width=600&height=100&lines=Mongodb+Assistant;7+Agents+%7C+12+Skills;Claude+Code+Plugin" alt="Mongodb Assistant" />

<br/>

<!-- Badge Row 1: Status Badges -->
[![Version](https://img.shields.io/badge/Version-2.0.0-blue?style=for-the-badge)](https://github.com/pluginagentmarketplace/custom-plugin-mongodb/releases)
[![License](https://img.shields.io/badge/License-Custom-yellow?style=for-the-badge)](LICENSE)
[![Status](https://img.shields.io/badge/Status-Production-brightgreen?style=for-the-badge)](#)
[![SASMP](https://img.shields.io/badge/SASMP-v1.3.0-blueviolet?style=for-the-badge)](#)

<!-- Badge Row 2: Content Badges -->
[![Agents](https://img.shields.io/badge/Agents-7-orange?style=flat-square&logo=robot)](#-agents)
[![Skills](https://img.shields.io/badge/Skills-12-purple?style=flat-square&logo=lightning)](#-skills)
[![Commands](https://img.shields.io/badge/Commands-4-green?style=flat-square&logo=terminal)](#-commands)

<br/>

<!-- Quick CTA Row -->
[📦 **Install Now**](#-quick-start) · [🤖 **Explore Agents**](#-agents) · [📖 **Documentation**](#-documentation) · [⭐ **Star this repo**](https://github.com/pluginagentmarketplace/custom-plugin-mongodb)

---

### What is this?

> **Mongodb Assistant** is a Claude Code plugin with **7 agents** and **12 skills** for mongodb development.

</div>

---

## 📑 Table of Contents

<details>
<summary>Click to expand</summary>

- [Quick Start](#-quick-start)
- [Features](#-features)
- [Agents](#-agents)
- [Skills](#-skills)
- [Commands](#-commands)
- [Documentation](#-documentation)
- [Contributing](#-contributing)
- [License](#-license)

</details>

---

## 🚀 Quick Start

### Prerequisites

- Claude Code CLI v2.0.27+
- Active Claude subscription

### Installation (Choose One)

<details open>
<summary><strong>Option 1: From Marketplace (Recommended)</strong></summary>

```bash
# Step 1️⃣ Add the marketplace
/plugin add marketplace pluginagentmarketplace/custom-plugin-mongodb

# Step 2️⃣ Install the plugin
/plugin install mongodb-developer-plugin@pluginagentmarketplace-mongodb

# Step 3️⃣ Restart Claude Code
# Close and reopen your terminal/IDE
```

</details>

<details>
<summary><strong>Option 2: Local Installation</strong></summary>

```bash
# Clone the repository
git clone https://github.com/pluginagentmarketplace/custom-plugin-mongodb.git
cd custom-plugin-mongodb

# Load locally
/plugin load .

# Restart Claude Code
```

</details>

### ✅ Verify Installation

After restart, you should see these agents:

```
mongodb-developer-plugin:04-mongodb-performance-indexing
mongodb-developer-plugin:02-mongodb-queries-aggregation
mongodb-developer-plugin:05-mongodb-replication-sharding
mongodb-developer-plugin:03-mongodb-data-modeling
mongodb-developer-plugin:01-mongodb-fundamentals
... and 2 more
```

---

## ✨ Features

| Feature | Description |
|---------|-------------|
| 🤖 **7 Agents** | Specialized AI agents for mongodb tasks |
| 🛠️ **12 Skills** | Reusable capabilities with Golden Format |
| ⌨️ **4 Commands** | Quick slash commands |
| 🔄 **SASMP v1.3.0** | Full protocol compliance |

---

## 🤖 Agents

### 7 Specialized Agents

| # | Agent | Purpose |
|---|-------|---------|
| 1 | **04-mongodb-performance-indexing** | Master MongoDB performance optimization, indexing strategies |
| 2 | **02-mongodb-queries-aggregation** | Master MongoDB query language, aggregation pipelines, and co |
| 3 | **05-mongodb-replication-sharding** | Master MongoDB replication, sharding, clustering, and high a |
| 4 | **03-mongodb-data-modeling** | Master MongoDB data modeling, schema design, and document st |
| 5 | **01-mongodb-fundamentals** | Expert in MongoDB fundamentals, document-oriented data model |
| 6 | **07-mongodb-application-development** | Master MongoDB application development with drivers and fram |
| 7 | **06-mongodb-security-administration** | Master MongoDB security, authentication, authorization, and  |

---

## 🛠️ Skills

### Available Skills

| Skill | Description | Invoke |
|-------|-------------|--------|
| `mongodb-crud` | Master MongoDB CRUD operations, document insertion, querying | `Skill("mongodb-developer-plugin:mongodb-crud")` |
| `mongodb-authentication` | Master MongoDB authentication methods including SCRAM, X.509 | `Skill("mongodb-developer-plugin:mongodb-authentication")` |
| `mongodb-index-creation` | Master MongoDB index creation and types. Learn single-field, | `Skill("mongodb-developer-plugin:mongodb-index-creation")` |
| `mongodb-integration` | Master MongoDB integration in applications with Node.js, Pyt | `Skill("mongodb-developer-plugin:mongodb-integration")` |
| `mongodb-security` | Master MongoDB security, authentication, authorization, encr | `Skill("mongodb-developer-plugin:mongodb-security")` |
| `mongodb-aggregation` | Master MongoDB aggregation pipeline for complex data transfo | `Skill("mongodb-developer-plugin:mongodb-aggregation")` |
| `mongodb-schema-design` | Master MongoDB schema design and data modeling patterns. Lea | `Skill("mongodb-developer-plugin:mongodb-schema-design")` |
| `mongodb-atlas-setup` | Master MongoDB Atlas cloud setup, cluster configuration, sec | `Skill("mongodb-developer-plugin:mongodb-atlas-setup")` |
| `mongodb-replication` | Master MongoDB replication, replica sets, and sharding for d | `Skill("mongodb-developer-plugin:mongodb-replication")` |
| `mongodb-indexing` | Master MongoDB indexing and query optimization. Learn index  | `Skill("mongodb-developer-plugin:mongodb-indexing")` |
| ... | +2 more | See skills/ directory |

---

## ⌨️ Commands

| Command | Description |
|---------|-------------|
| `/deployment-guide` | guide - MongoDB Deployment Strategies |
| `/learn-mongodb` | mongodb - Start Your MongoDB Learning Journey |
| `/query-builder` | builder - MongoDB Query Helper |
| `/tutorial` | MongoDB Tutorials & Walkthroughs |

---

## 📚 Documentation

| Document | Description |
|----------|-------------|
| [CHANGELOG.md](CHANGELOG.md) | Version history |
| [CONTRIBUTING.md](CONTRIBUTING.md) | How to contribute |
| [LICENSE](LICENSE) | License information |

---

## 📁 Project Structure

<details>
<summary>Click to expand</summary>

```
custom-plugin-mongodb/
├── 📁 .claude-plugin/
│   ├── plugin.json
│   └── marketplace.json
├── 📁 agents/              # 7 agents
├── 📁 skills/              # 12 skills (Golden Format)
├── 📁 commands/            # 4 commands
├── 📁 hooks/
├── 📄 README.md
├── 📄 CHANGELOG.md
└── 📄 LICENSE
```

</details>

---

## 📅 Metadata

| Field | Value |
|-------|-------|
| **Version** | 2.0.0 |
| **Last Updated** | 2025-12-29 |
| **Status** | Production Ready |
| **SASMP** | v1.3.0 |
| **Agents** | 7 |
| **Skills** | 12 |
| **Commands** | 4 |

---

## 🤝 Contributing

Contributions are welcome! Please read our [Contributing Guide](CONTRIBUTING.md).

1. Fork the repository
2. Create your feature branch
3. Follow the Golden Format for new skills
4. Submit a pull request

---

## ⚠️ Security

> **Important:** This repository contains third-party code and dependencies.
>
> - ✅ Always review code before using in production
> - ✅ Check dependencies for known vulnerabilities
> - ✅ Follow security best practices
> - ✅ Report security issues privately via [Issues](../../issues)

---

## 📝 License

Copyright © 2025 **Dr. Umit Kacar** & **Muhsin Elcicek**

Custom License - See [LICENSE](LICENSE) for details.

---

## 👥 Contributors

<table>
<tr>
<td align="center">
<strong>Dr. Umit Kacar</strong><br/>
Senior AI Researcher & Engineer
</td>
<td align="center">
<strong>Muhsin Elcicek</strong><br/>
Senior Software Architect
</td>
</tr>
</table>

---

<div align="center">

**Made with ❤️ for the Claude Code Community**

[![GitHub](https://img.shields.io/badge/GitHub-pluginagentmarketplace-black?style=for-the-badge&logo=github)](https://github.com/pluginagentmarketplace)

</div>
