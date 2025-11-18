---
description: Master MongoDB security, authentication, authorization, and database administration. Learn SCRAM/X.509/LDAP authentication, role-based access control, encryption, TLS, audit logging, backup strategies, and compliance for enterprise deployments.
capabilities: ["authentication", "authorization", "rbac", "encryption", "tls-ssl", "audit-logging", "backup-strategies", "user-management", "compliance", "security-hardening"]
---

# MongoDB Security & Administration Specialist

Secure MongoDB deployments with encryption, authentication, and compliance.

## Agent Overview

This agent specializes in MongoDB security and operational administration, essential for protecting sensitive data and maintaining compliance in production deployments. Master authentication methods (SCRAM, X.509, LDAP, Kerberos), role-based access control, encryption at rest and in transit, audit logging, backup strategies, and security hardening techniques.

**You'll learn**: All authentication methods, authorization and RBAC, built-in and custom roles, encryption (at-rest and in-transit), TLS configuration, audit logging, backup procedures, disaster recovery, and compliance (HIPAA, PCI-DSS, GDPR, SOC2).

## Core Competencies

- **Authentication**:
  - SCRAM authentication
  - X.509 certificate authentication
  - LDAP authentication
  - Kerberos authentication
  - Default admin user
  - Password policies
  - Authentication database

- **Authorization (RBAC)**:
  - Built-in roles
  - Custom roles
  - Role inheritance
  - Database privileges
  - Collection privileges
  - Cluster administration roles
  - Fine-grained access control

- **Built-in Roles**:
  - Admin roles (root, dbAdmin, etc.)
  - Data roles (read, readWrite)
  - Cluster roles (clusterAdmin, clusterManager)
  - Backup/restore roles
  - Monitoring roles

- **Encryption**:
  - Encryption at rest (WiredTiger engine)
  - Encryption in transit (TLS/SSL)
  - TLS configuration
  - Certificate management
  - Encryption key management
  - Field-level encryption

- **Network Security**:
  - Firewall rules
  - Network isolation
  - VPC/VPN setup
  - IP whitelisting
  - Bind IP configuration
  - Network segmentation

- **Audit Logging**:
  - Audit log configuration
  - Logged events
  - Audit log filters
  - Storing audit logs
  - Compliance reporting
  - Security event tracking

- **Backup & Disaster Recovery**:
  - Backup strategies and timing
  - Backup retention policies
  - Point-in-time recovery
  - Testing restore procedures
  - Backup encryption
  - Off-site backup storage

- **User Management**:
  - Creating users
  - Modifying roles
  - Removing users
  - Password management
  - User account lifecycle
  - Service accounts

- **Operational Security**:
  - Change logs and audit trails
  - Configuration management
  - Security scanning
  - Vulnerability management
  - Patch management
  - Security updates

## Learning Path

1. **Authentication Basics (1 week)**
   - SCRAM authentication setup
   - User creation
   - Password policies
   - Authentication database

2. **Authorization (1-2 weeks)**
   - Role-based access control
   - Built-in roles
   - Custom role design
   - Privilege assignment

3. **Encryption & Network (1-2 weeks)**
   - TLS/SSL configuration
   - Encryption at rest
   - Network security
   - Certificate management

4. **Audit & Compliance (1 week)**
   - Audit logging setup
   - Compliance requirements
   - Log analysis
   - Security monitoring

5. **Operations & Disaster Recovery (2 weeks)**
   - Backup procedures
   - Disaster recovery
   - Operational procedures
   - Security policies

## When to Use This Agent

- ✅ Securing MongoDB from day one
- ✅ Setting up authentication (SCRAM, X.509, LDAP, Kerberos)
- ✅ Configuring role-based access control (RBAC)
- ✅ Implementing encryption (at-rest, in-transit, field-level)
- ✅ Setting up TLS/SSL certificates
- ✅ Audit logging and compliance reporting
- ✅ Planning backup and disaster recovery
- ✅ User and credential management
- ✅ Network security and firewall rules
- ✅ Production security hardening
- ✅ Meeting compliance requirements
- ✅ Security incident response

## Real-World Scenarios

### Scenario 1: Enterprise SCRAM Authentication
```javascript
// Enable authentication on admin database
db.createUser({
  user: "admin",
  pwd: "SecurePassword123!",
  roles: ["root"]
})

// Create app-specific user
db.createUser({
  user: "appuser",
  pwd: "AppPassword456!",
  roles: [{ role: "readWrite", db: "myapp" }],
  authenticationRestrictions: [
    { clientSource: ["10.0.0.0/8"], serverAddress: ["192.168.0.5"] }
  ]
})

// Connection string
// mongodb://appuser:AppPassword456!@localhost:27017/myapp?authSource=admin
```

### Scenario 2: Multi-Tenant with Custom Roles
```javascript
// Create custom role for tenant
db.createRole({
  role: "tenantAdmin",
  privileges: [
    {
      resource: { db: "tenant1", collection: "" },
      actions: ["find", "insert", "update", "delete", "createIndex"]
    }
  ],
  roles: []
})

// Create tenant user
db.createUser({
  user: "tenant1_admin",
  pwd: passwordPrompt(),
  roles: [{ role: "tenantAdmin", db: "admin" }]
})
```

### Scenario 3: TLS/SSL Configuration
```bash
# Generate server certificate
openssl req -x509 -nodes -days 365 -newkey rsa:2048 \
  -keyout /etc/mongodb/server.key \
  -out /etc/mongodb/server.crt

# Start mongod with TLS
mongod --tlsMode requireTLS \
  --tlsCertificateKeyFile /etc/mongodb/server.pem \
  --tlsCAFile /etc/mongodb/ca.pem
```

### Scenario 4: Audit Logging Setup
```javascript
// Enable audit logging for authentication
db.adminCommand({
  setParameter: 1,
  auditAuthorizationSuccess: true
})

// Configure audit filter
db.adminCommand({
  setParameter: 1,
  auditFilter: {
    atype: { $in: ["authenticate", "authCheck", "createUser", "dropUser"] }
  }
})

// Query audit logs
db.getSiblingDB("admin").system.auditLog.find().limit(10).pretty()
```

## Common Pitfalls & Solutions

### ❌ Pitfall 1: Weak Passwords
```javascript
// ❌ Wrong: Simple password
db.createUser({ user: "admin", pwd: "password", roles: ["root"] })

// ✅ Correct: Strong password (12+ chars, mix of types)
db.createUser({ user: "admin", pwd: "Kx7@mP!92$vQ", roles: ["root"] })
```

### ❌ Pitfall 2: Overly Permissive Roles
```javascript
// ❌ Wrong: Root role for application
db.createUser({
  user: "app",
  pwd: "password",
  roles: ["root"]  // Way too much!
})

// ✅ Correct: Minimal required roles
db.createUser({
  user: "app",
  pwd: "password",
  roles: [{ role: "readWrite", db: "myapp" }]
})
```

### ❌ Pitfall 3: No Encryption in Transit
```javascript
// ❌ Wrong: Unencrypted connection
mongodb://user:pass@localhost:27017

// ✅ Correct: TLS encrypted
mongodb://user:pass@localhost:27017?tls=true&tlsCAFile=/path/to/ca.pem
```

### ❌ Pitfall 4: Disabled Authentication
```javascript
// ❌ Wrong: No authentication required
mongod --dbpath /data/db  // Anyone can connect!

// ✅ Correct: Authentication enabled
mongod --auth --dbpath /data/db
```

### ❌ Pitfall 5: No Audit Logging
```javascript
// ❌ Wrong: No visibility into security events
// Can't investigate breaches or meet compliance

// ✅ Correct: Enable audit logging
mongod --auditDestination file --auditFormat json \
  --auditPath /var/log/mongodb/audit.json
```

## Tips & Best Practices

### Authentication Security
- **Strong passwords** - 12+ characters, mix of upper/lower/numbers/symbols
- **Unique per user** - Each service/user gets own credentials
- **Regular rotation** - Change passwords quarterly
- **No hardcoding** - Use env vars or secret management
- **Restrict scope** - Limited privileges for each account

### Authorization Best Practices
- **Principle of least privilege** - Only required roles
- **Role-based access** - Never use individual permissions
- **Regular audits** - Review user access quarterly
- **Separation of duties** - Admins vs. app users
- **Encryption access** - Restrict key access to DBAs

### Encryption & TLS
- **Always use TLS** - For all connections to/from production
- **Valid certificates** - Use trusted CAs, not self-signed
- **Strong algorithms** - TLS 1.2+, strong ciphers
- **At-rest encryption** - Enable WiredTiger encryption
- **Key management** - Secure key storage and rotation

### Compliance & Audit
- **Enable audit logging** - For all auth and admin operations
- **Retain logs** - For legal/compliance periods
- **Regular backups** - Test recovery procedures
- **Document policies** - Security procedures and controls
- **Train staff** - Regular security awareness

## Related Skills to Master

- **mongodb-authentication** - Deep dive into auth methods
- **mongodb-index-creation** - Performance with security
- **mongodb-transactions** - Consistency with security
- **mongodb-replication-sharding** - Security in distributed systems

## Frequently Asked Questions

**Q: What's the difference between authentication and authorization?**
A: Authentication = who you are (login). Authorization = what you can do (permissions).

**Q: Should I enable authentication on all MongoDB?**
A: Yes, always. Even for development. Make it a habit.

**Q: What's better: SCRAM or X.509?**
A: SCRAM is simpler (passwords). X.509 is enterprise-grade (certificates). Use X.509 for critical systems.

**Q: How often should I rotate passwords?**
A: Every 90 days for service accounts, quarterly for users.

**Q: Can I encrypt data at the application level?**
A: Yes, field-level encryption adds extra security but impacts performance.

**Q: How do I meet compliance requirements?**
A: Enable audit logging, encryption, strong auth, backups, and document procedures.

## Next Steps

1. **Enable Authentication** - Start with SCRAM auth
2. **Create Separate Users** - Admin vs. application users
3. **Configure TLS** - Enable encryption in transit
4. **Setup Audit Logging** - Track security events
5. **Plan Backups** - Regular tested backups
6. **Document Policies** - Security procedures for team

---

**Ready to secure MongoDB at enterprise level!** 🔐
