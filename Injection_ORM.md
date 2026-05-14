## Advanced Injection Attacks — Object-Relational Mapping (ORM)

## Objective
Understand ORM behavior to:
- Identify hidden injection points
- Exploit unsafe query construction
- Bypass assumptions of “safe by default”

Think: *“Is the ORM really protecting the query—or just abstracting it?”*

---

## 1. What is ORM

ORM = layer between code and database

- Converts objects → SQL queries
- Used in frameworks:
  - Sequelize (Node.js)
  - Hibernate (Java)
  - Django ORM (Python)

---

## 2. Example

### Code

```javascript
User.findOne({
  where: { username: input }
})
```

---

### Generated SQL

```sql
SELECT * FROM users WHERE username = 'input'
```

---

## 3. Why ORM Can Be Vulnerable

- Developers assume ORM = safe
- Unsafe usage reintroduces SQLi
- Raw queries often used

---

## 4. Common Vulnerable Patterns

### A. Raw queries

```javascript
db.query("SELECT * FROM users WHERE name = '" + input + "'")
```

→ Direct SQL injection

---

### B. Unsafe string concatenation

```javascript
User.findOne({
  where: "username = '" + input + "'"
})
```

---

### C. Dynamic query building

```javascript
whereClause = "username = '" + input + "'"
```

---

### D. Operator injection (NoSQL-style ORM)

```json
{"username": {"$ne": null}}
```

---

## 5. Injection via ORM Filters

### Example (Sequelize-like)

```json
{"where": {"username": "admin"}}
```

---

### Inject operator

```json
{"where": {"username": {"$ne": null}}}
```

→ Bypass condition

---

## 6. Query Manipulation

### Modify conditions

```json
{"where": {"id": 1}}
→
{"where": {"id": {"$gt": 0}}}
```

---

### Add unexpected fields

```json
{"where": {"isAdmin": true}}
```

---

## 7. Blind Injection via ORM

If no output:

- Boolean-based
- Time-based (if raw queries used)

---

## 8. Indicators of Vulnerability

- ORM queries behave like raw SQL
- Injection works despite ORM usage
- Unexpected results from modified input
- Operators accepted in input

---

## 9. Fast Workflow (Exam Mode)

1. Identify ORM usage (framework clues)
2. Test input:
   - quotes (`'`)
3. Try:
   - operator injection (`$ne`, `$gt`)
4. Check for raw queries
5. Observe:
   - errors
   - unexpected matches
6. Confirm injection

---

## 10. Mental Model

ORM is abstraction, not protection.

If developer:
- concatenates strings
- allows operators
- uses raw queries

→ ORM becomes transparent  
→ And injection is still possible