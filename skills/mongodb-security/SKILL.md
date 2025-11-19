---
name: mongodb-security-admin
description: Master MongoDB security, authentication, authorization, encryption, and backup. Learn role-based access control, TLS/SSL, encryption, and disaster recovery. Use when securing deployments, managing users, or implementing compliance.
---

# MongoDB Security & Administration

Master MongoDB security and operational practices.

## Quick Start

### Enable Authentication
```bash
# Create admin user before enabling auth
mongod --dbpath /data --logpath /var/log/mongod.log

mongo
> use admin
> db.createUser({
    user: 'admin',
    pwd: 'securepassword',
    roles: ['root']
  })

# Restart with auth enabled
mongod --auth --dbpath /data
```

### Authentication
```javascript
// Connect with credentials
const client = new MongoClient(
  'mongodb://admin:password@localhost:27017/?authSource=admin'
);

// Create application user
db.createUser({
  user: 'appuser',
  pwd: 'apppassword',
  roles: ['readWrite']
})
```

### Authorization (RBAC)

```javascript
// Built-in roles
/*
Admin Roles:
  - root: Full access
  - dbAdmin: Database administration
  - userAdmin: User management

Database Roles:
  - read: Read-only access
  - readWrite: Read and write

Cluster Roles:
  - clusterAdmin: Full cluster access
  - clusterManager: Cluster monitoring
  - clusterMonitor: Read-only monitoring
*/

// Create user with specific role
db.createUser({
  user: 'analyst',
  pwd: 'password',
  roles: [{ role: 'read', db: 'analytics' }]
})

// Grant multiple roles
db.grantRolesToUser('analyst', [
  { role: 'read', db: 'db1' },
  { role: 'readWrite', db: 'db2' }
])
```

### Custom Roles
```javascript
// Create custom role
db.createRole({
  role: 'reportViewer',
  privileges: [
    {
      resource: { db: 'analytics', collection: 'reports' },
      actions: ['find']
    }
  ],
  roles: []
})

// Assign custom role
db.grantRolesToUser('analyst', [
  { role: 'reportViewer', db: 'admin' }
])
```

## Encryption

### TLS/SSL Setup
```bash
# Generate self-signed certificate
openssl req -newkey rsa:2048 -new -x509 -days 365 -nodes \
  -out server.crt -keyout server.key
cat server.crt server.key > server.pem

# Start MongoDB with TLS
mongod --tlsMode requireTLS \
  --tlsCertificateKeyFile /path/to/server.pem \
  --dbpath /data

# Connect with TLS
mongo --tls --tlsCertificateKeyFile /path/to/client.pem \
  mongodb://localhost:27017
```

### Encryption at Rest
```bash
# Enable encryption with WiredTiger
mongod --encryptionCipherMode AES256-CBC \
  --encryptionKeyFile /path/to/keyfile.key \
  --dbpath /data
```

## Backup & Recovery

### Backup Methods
```bash
# Backup with mongodump
mongodump --out /backup/`date +%Y%m%d`

# Backup specific database
mongodump --db myapp --out /backup/myapp

# Backup with compression
mongodump --archive=backup.archive --gzip

# Restore
mongorestore /backup/
mongorestore --archive=backup.archive --gzip
```

### Point-in-Time Recovery with Snapshots
```bash
# Snapshot strategy
# 1. Create filesystem snapshot on replica secondary
# 2. Stop mongod
# 3. Copy snapshot to backup
# 4. Restart mongod
# 5. Use oplog for point-in-time recovery

# Restore from snapshot
# 1. Restore filesystem snapshot
# 2. Start mongod (automatic recovery)
# 3. Verify data integrity
```

## Audit Logging

```javascript
// Enable audit logging
// mongod --auditLog.destination file \
//   --auditLog.format BSON \
//   --auditLog.path /var/log/mongodb/audit.log

// View audit log
db.adminCommand({
  getParameter: 1,
  auditLog: 1
})

// Configure audit filters
// --auditLog.filter '{ atype: "authenticate" }'
```

## User Management

```javascript
// List users
db.getUsers()

// Modify user password
db.changeUserPassword('username', 'newpassword')

// Remove user
db.removeUser('username')

// Check current user
db.runCommand({ connectionStatus: 1 })
```

## Python Examples

```python
from pymongo import MongoClient
from pymongo.auth_mechanisms import MECHANISMS

# Connect with authentication
client = MongoClient(
    'mongodb://user:password@localhost:27017/?authSource=admin'
)

# Create user
db = client['admin']
db.command('createUser', 'newuser',
    pwd='password',
    roles=['readWrite']
)

# Check permissions
db.command('usersInfo', 'newuser')
```

## Security Checklist

✅ Enable authentication in production
✅ Use strong, unique passwords
✅ Implement role-based access control
✅ Enable TLS/SSL for connections
✅ Enable encryption at rest
✅ Implement audit logging
✅ Regular backup testing
✅ Network isolation (firewall)
✅ IP whitelisting
✅ Principle of least privilege
✅ Monitor access logs
✅ Keep MongoDB updated
