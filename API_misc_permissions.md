## API — Misconfigured Permissions

## Objective
Identify broken or missing access controls allowing:
- Unauthorized data access
- Privilege escalation
- Cross-user data exposure

Think: *“Can I access or modify something I shouldn’t?”*

---

## 1. Types of Permission Issues

### A. Horizontal Access Control (IDOR)
Access other users’ data at same privilege level

Example:
```http
GET /api/user/123   → your data
GET /api/user/124   → someone else’s data
```

---

### B. Vertical Access Control
Access higher privilege actions (admin)

Example:
```http
GET /api/admin/users
POST /api/admin/deleteUser
```

---

### C. Missing Authorization Check
Endpoint accessible without any auth

```http
GET /api/internal/stats
```

---

### D. Function-Level Access Control
Sensitive functions exposed without proper restriction

```http
POST /api/resetPassword
POST /api/makeAdmin
```

---

## 2. Where to Test

- All API endpoints (especially hidden ones)
- ID-based endpoints:
```
/user/{id}
/order/{id}
/invoice/{id}
```

- Admin / internal routes:
```
/admin/
/internal/
/debug/
```

---

## 3. Basic Testing Approach

### A. Remove authentication

```http
Authorization: Bearer <token> → remove
```

→ Does it still work?

---

### B. Use another user’s ID

```http
GET /api/user/1 → GET /api/user/999
```

---

### C. Use low-priv user on admin endpoint

```http
GET /api/admin/dashboard
```

---

### D. Change roles manually

```json
{"role": "user"} → {"role": "admin"}
```

---

## 4. IDOR Testing (High Yield)

### Step-by-step

1. Find ID parameter:
```http
GET /api/order?id=1001
```

2. Modify:
```http
?id=1002
?id=9999
```

3. Observe:
- Data changes?
- Other users’ data exposed?

---

### Common ID fields

```json
"id"
"user_id"
"account_id"
"order_id"
```

---

### Hidden IDOR (in JSON)

```json
{"user_id": 1}
→
{"user_id": 2}
```

---

## 5. Privilege Escalation

### A. Direct endpoint access

```http
POST /api/admin/createUser
```

---

### B. Role injection

```json
{"isAdmin": true}
{"role": "admin"}
```

---

### C. Parameter tampering

```json
{"permissions": ["user"]}
→
{"permissions": ["admin"]}
```

---

## 6. Token / Session Issues

### Test:

- Expired token still valid?
- Token reusable after logout?
- Token usable across accounts?

---

### Token swapping

Use another user’s token:

```http
Authorization: Bearer TOKEN_USER_B
```

→ Can you access their data?

---

## 7. Common Vulnerabilities

- No ownership validation
- Trusting client-side roles
- Missing checks on sensitive endpoints
- Inconsistent checks across endpoints

---

## 8. Indicators of Vulnerability

- Access granted when it shouldn’t
- Data from other users returned
- No difference between admin/user responses
- “200 OK” instead of “403 Forbidden”

---

## 9. Fast Workflow (Exam Mode)

1. Map endpoints (Burp, JS, docs)
2. Identify ID parameters
3. Test IDOR:
   - change IDs
4. Test auth:
   - remove token
   - downgrade user
5. Test admin endpoints
6. Modify roles/permissions in requests
7. Compare responses carefully

---

## 10. Mental Model

APIs often assume:
- “User will only request their own data”
- “Frontend enforces permissions”

Both are wrong.

Your job:
→ Change identity, role, or ID → See what breaks