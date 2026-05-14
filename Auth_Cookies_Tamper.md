## Session Management Testing — Sessions, Cookies & Tampering

## Objective
Understand and test how sessions are handled to:
- Hijack sessions
- Manipulate identity
- Bypass authentication

Think: *“Can I change or reuse what identifies the user?”*

---

## 1. Session Management & Session IDs

### What is a Session ID

- Unique identifier for a user session
- Generated after login

Example:

```http
Cookie: session=abc123
```

---

### Key properties

- Random (not predictable)
- Unique per user
- Tied to server-side session

---

### Common issues

- Predictable session IDs
- Reusable sessions
- No expiration
- Same session before/after login

---

## 2. Cookies & Cookie Attributes

### Basic cookie

```http
Set-Cookie: session=abc123
```

---

### Important attributes

#### HttpOnly
```http
HttpOnly
```
- Prevents JavaScript access

---

#### Secure
```http
Secure
```
- Sent only over HTTPS

---

#### SameSite

```http
SameSite=Strict
SameSite=Lax
SameSite=None
```

- Controls cross-site requests

---

### Example

```http
Set-Cookie: session=abc123; HttpOnly; Secure; SameSite=Lax
```

---

## 3. Testing Session Management Schema

### A. Session fixation

1. Get session before login
2. Login with same session
3. Check if session ID changes

→ If not → vulnerable

---

### B. Session reuse

- Logout
- Reuse same cookie

→ Still valid?

---

### C. Multiple sessions

- Login from multiple devices
- Same session reused?

---

### D. Session expiration

- Leave idle
- Use session later

→ Still active?

---

## 4. Cookie Tampering

### A. Modify session ID

```http
session=abc123 → session=abc124
```

→ Access changes?

---

### B. Predict session

Try patterns:

```text
session=1
session=2
session=1000
```

---

### C. Modify encoded cookies

If Base64:

```text
dXNlcj11c2Vy → user=user
```

→ Change:

```text
user=admin
```

---

### D. Add fields

```http
Cookie: role=user → role=admin
```

---

### E. Remove cookie

```http
Cookie: (empty)
```

→ Still authenticated?

---

## 5. Common Vulnerabilities

- Session ID in URL
- No regeneration after login
- Weak/random session IDs
- Client-side session storage
- Trusting cookie data

---

## 6. Indicators of Vulnerability

- Session remains valid after logout
- Session ID predictable
- Modified cookie changes behavior
- Same session used across users

---

## 7. Fast Workflow (Exam Mode)

1. Capture session cookie
2. Test:
   - reuse after logout
   - reuse after time
3. Modify cookie:
   - change value
   - decode/encode
4. Check attributes:
   - HttpOnly
   - Secure
   - SameSite
5. Test fixation:
   - login with same session
6. Observe:
   - access changes
   - persistence

---

## 8. Mental Model

Session = identity token

If you can:
→ Predict it  
→ Steal it  
→ Modify it  

You become the user