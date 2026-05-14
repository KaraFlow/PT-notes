## Authentication Testing — Parameter Manipulation & JWT Attacks

## Objective
Bypass authentication by:
- Manipulating request parameters
- Exploiting JWT weaknesses
- Abusing trust in client-controlled data

Think: *“What part of authentication is trusted but shouldn’t be?”*

---

## 1. Bypassing Authentication — Parameter Manipulation

### A. Modify authentication flags

```json
{"authenticated": false} → {"authenticated": true}
{"loggedIn": false} → {"loggedIn": true}
```

---

### B. Inject role / privilege

```json
{"role": "user"} → {"role": "admin"}
{"isAdmin": false} → {"isAdmin": true}
```

---

### C. Remove authentication parameter

```json
{"token": "abc123"} → {}
```

→ Does it still work?

---

### D. Use alternate parameters

```json
{"user":"admin"}
{"username":"admin"}
{"email":"admin@test.com"}
```

---

### E. Header-based trust

```http
X-User: admin
X-Role: admin
X-Admin: true
```

---

### Indicators

- Access granted without valid credentials
- Role escalation works
- Authentication bypassed via request manipulation

---

## 2. JSON Web Tokens (JWT)

### Structure

```
header.payload.signature
```

Example:

```
eyJhbGciOiJIUzI1NiJ9.eyJ1c2VyIjoiYWRtaW4ifQ.signature
```

---

### Decode JWT

- Base64 decode header & payload

```json
{
  "alg": "HS256"
}
```

```json
{
  "user": "admin"
}
```

---

## 3. The None Algorithm Vulnerability

### Concept

Change algorithm to `none` → remove signature verification

---

### Attack

1. Decode JWT
2. Modify header:

```json
{"alg": "none"}
```

3. Modify payload:

```json
{"user": "admin"}
```

4. Remove signature

```
header.payload.
```

---

### Result

- If server accepts → full bypass

---

## 4. Exposed Claims

### Common claims

```json
{
  "user": "admin",
  "role": "user",
  "isAdmin": false,
  "exp": 1710000000
}
```

---

### Manipulation

```json
"isAdmin": false → true
"role": "user" → "admin"
```

---

### Re-sign (if weak key)

- Try known secrets:

```text
secret
admin
123456
```

---

### Tools

- jwt.io
- Burp JWT Editor

---

## 5. Other JWT Weaknesses

### A. No signature verification
- Token accepted without validation

---

### B. Weak secret key
- Can brute-force and re-sign

---

### C. Expiration bypass

```json
"exp": 9999999999
```

---

### D. Token reuse

- Old tokens still valid

---

## 6. Indicators of Vulnerability

- Modified token still accepted
- Role escalation via token
- No signature validation errors
- Expired tokens still work

---

## 7. Fast Workflow (Exam Mode)

1. Capture JWT
2. Decode:
   - header
   - payload
3. Modify:
   - role / isAdmin
4. Try:
   - alg: none
   - remove signature
5. If fails:
   - try weak secrets
6. Replay token
7. Observe access

---

## 8. Mental Model

JWT is:
- Client-controlled data
- Trusted by server

If validation is weak:
→ You control identity  
→ You control privileges