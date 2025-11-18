---
name: security-best-practices
description: Master security principles, secure coding, testing strategies, and compliance requirements. Learn to build secure applications, identify vulnerabilities, and meet regulatory standards. Use when implementing security features, conducting security reviews, or achieving compliance.
---

# Security & Best Practices Skill

Master security with comprehensive guidance on secure development and testing.

## Quick Start

### Secure Password Handling
```python
import bcrypt
from cryptography.fernet import Fernet

# Password hashing (don't store plain passwords!)
def hash_password(password):
    salt = bcrypt.gensalt(rounds=12)
    return bcrypt.hashpw(password.encode('utf-8'), salt)

def verify_password(password, hashed):
    return bcrypt.checkpw(password.encode('utf-8'), hashed)

# Encryption for sensitive data
def encrypt_data(data):
    key = Fernet.generate_key()
    cipher = Fernet(key)
    encrypted = cipher.encrypt(data.encode('utf-8'))
    return encrypted

def decrypt_data(encrypted_data, key):
    cipher = Fernet(key)
    return cipher.decrypt(encrypted_data).decode('utf-8')
```

### Input Validation & Sanitization
```javascript
// Always validate and sanitize user input
function validateEmail(email) {
    const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
    return emailRegex.test(email);
}

// Prevent XSS attacks
function sanitizeHTML(input) {
    const div = document.createElement('div');
    div.textContent = input; // Sets as text, not HTML
    return div.innerHTML;
}

// Prevent SQL injection (use parameterized queries)
const query = 'SELECT * FROM users WHERE email = ?';
db.query(query, [userEmail]); // ✓ Safe

// NOT: db.query(`SELECT * FROM users WHERE email = '${userEmail}'`); // ✗ Vulnerable
```

### Authentication Implementation
```python
from fastapi import FastAPI, Depends, HTTPException, status
from passlib.context import CryptContext
from datetime import datetime, timedelta
import jwt

app = FastAPI()
pwd_context = CryptContext(schemes=["bcrypt"])
SECRET_KEY = "your-secret-key-change-this"

class User(BaseModel):
    username: str
    email: str
    password: str

def create_access_token(data, expires_delta=None):
    to_encode = data.copy()
    if expires_delta:
        expire = datetime.utcnow() + expires_delta
    else:
        expire = datetime.utcnow() + timedelta(hours=1)

    to_encode.update({"exp": expire})
    encoded_jwt = jwt.encode(to_encode, SECRET_KEY, algorithm="HS256")
    return encoded_jwt

@app.post("/login")
async def login(user: User):
    # Verify credentials
    hashed_password = pwd_context.hash(user.password)
    # ... validate user ...

    access_token = create_access_token(data={"sub": user.username})
    return {"access_token": access_token, "token_type": "bearer"}

async def get_current_user(token: str = Depends(oauth2_scheme)):
    try:
        payload = jwt.decode(token, SECRET_KEY, algorithms=["HS256"])
        username = payload.get("sub")
        if username is None:
            raise HTTPException(status_code=401)
    except jwt.InvalidTokenError:
        raise HTTPException(status_code=401)
    return {"username": username}
```

### HTTPS and Security Headers
```javascript
// Express.js security headers
const express = require('express');
const helmet = require('helmet');
const app = express();

// Apply security headers
app.use(helmet());

// Additional headers
app.use((req, res, next) => {
    res.setHeader('X-Content-Type-Options', 'nosniff');
    res.setHeader('X-Frame-Options', 'DENY');
    res.setHeader('X-XSS-Protection', '1; mode=block');
    res.setHeader('Strict-Transport-Security', 'max-age=31536000; includeSubDomains');
    next();
});

// CSRF protection
const csrfProtection = csrf({ cookie: false });
app.post('/submit', csrfProtection, (req, res) => {
    // Handle form submission
});
```

### Unit Testing for Security
```python
import pytest
from app import hash_password, verify_password

class TestSecurity:
    def test_password_hashing(self):
        password = "SecurePassword123!"
        hashed = hash_password(password)

        assert hashed != password  # Never store plain
        assert verify_password(password, hashed)  # Verify works
        assert not verify_password("wrong", hashed)  # Wrong fails

    def test_input_validation(self):
        from validators import validate_email

        assert validate_email("user@example.com")
        assert not validate_email("invalid-email")
        assert not validate_email("")

    def test_sql_injection_prevention(self):
        from db import query

        # Test with parameterized query
        result = query("SELECT * FROM users WHERE id = ?", [123])
        assert result is not None
```

## Advanced Topics

### OWASP Top 10
- Injection attacks (SQL, command)
- Broken authentication
- Sensitive data exposure
- XML external entities (XXE)
- Broken access control
- Security misconfiguration
- XSS (Cross-Site Scripting)
- Insecure deserialization
- Using components with known vulnerabilities
- Insufficient logging

### Cryptography
- Encryption (symmetric, asymmetric)
- Hashing and salting
- Digital signatures
- TLS/SSL protocols
- Key management

### Security Testing
- Static analysis tools
- Dynamic application testing
- Penetration testing techniques
- Dependency vulnerability scanning
- SAST and DAST tools

### Compliance
- GDPR compliance
- CCPA requirements
- PCI DSS standards
- HIPAA regulations
- SOC 2 compliance

## Resources

- [OWASP Top 10](https://owasp.org/www-project-top-ten/)
- [CWE - Common Weakness Enumeration](https://cwe.mitre.org)
- [NIST Cybersecurity Framework](https://www.nist.gov/cyberframework)
- [HackerOne](https://www.hackerone.com)
- [PortSwigger Web Security Academy](https://portswigger.net/web-security)
