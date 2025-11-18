---
name: backend-technologies
description: Learn backend development frameworks, patterns, and best practices. Master Node.js, Python, Java, Go, and Rust. Use when building APIs, implementing authentication, or designing backend systems.
---

# Backend Technologies Skill

Master server-side development with comprehensive guidance on building scalable APIs and backend systems.

## Quick Start

### Express.js REST API
```javascript
const express = require('express');
const app = express();

app.use(express.json());

// User model
const users = [];

// GET /users
app.get('/users', (req, res) => {
  res.json(users);
});

// POST /users
app.post('/users', (req, res) => {
  const user = { id: Date.now(), ...req.body };
  users.push(user);
  res.status(201).json(user);
});

// GET /users/:id
app.get('/users/:id', (req, res) => {
  const user = users.find(u => u.id == req.params.id);
  user ? res.json(user) : res.status(404).json({ error: 'Not found' });
});

app.listen(3000);
```

### FastAPI (Python)
```python
from fastapi import FastAPI
from pydantic import BaseModel

app = FastAPI()

class User(BaseModel):
    id: int
    name: str
    email: str

users_db = []

@app.get('/users')
async def get_users():
    return users_db

@app.post('/users')
async def create_user(user: User):
    users_db.append(user)
    return user

@app.get('/users/{user_id}')
async def get_user(user_id: int):
    return next((u for u in users_db if u.id == user_id), None)
```

### Spring Boot (Java)
```java
@RestController
@RequestMapping("/api/users")
public class UserController {

    @GetMapping
    public List<User> getUsers() {
        return userService.getAllUsers();
    }

    @PostMapping
    public User createUser(@RequestBody User user) {
        return userService.save(user);
    }

    @GetMapping("/{id}")
    public User getUser(@PathVariable Long id) {
        return userService.findById(id);
    }
}
```

## Advanced Topics

### Database Integration
- ORM/ODM libraries (Sequelize, SQLAlchemy, JPA)
- Query optimization and indexing
- Connection pooling
- Transaction management

### Authentication & Authorization
- JWT implementation
- OAuth2 flows
- Password hashing (bcrypt, scrypt)
- Session management

### API Design
- RESTful principles
- API versioning strategies
- Rate limiting and throttling
- Request/response formatting

### Async Programming
- Promises and async/await
- Event loops and callbacks
- Stream processing
- Worker threads/processes

### Error Handling
- Centralized error handlers
- Error logging and monitoring
- HTTP status codes
- Error recovery strategies

## Resources

- [Express.js Docs](https://expressjs.com)
- [FastAPI Docs](https://fastapi.tiangolo.com)
- [Spring Boot Docs](https://spring.io/projects/spring-boot)
- [MDN Web Docs](https://developer.mozilla.org)
