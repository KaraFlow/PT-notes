## Session Management Testing — Session Hijacking & Fixation

## Objective
Exploit session handling flaws to:
- Take over user sessions (hijacking)
- Force users to use attacker-controlled sessions (fixation)

Think: *“Can I become the user without knowing their password?”*

---

## 1. Session Hijacking

### Concept
Steal a valid session ID and reuse it to impersonate a user

---

### A. How sessions get stolen

- XSS (read cookies)
- Network sniffing (HTTP, not HTTPS)
- Logs / referers
- Shared computers
- Insecure storage (localStorage)

---

### B. Exploitation

1. Obtain session ID:

```http
Cookie: session=abc123
```

2. Replay request:

```http
Cookie: session=abc123
```

→ Access as victim

---

### C. Testing

- Capture your session
- Replay in another browser
- Check if access granted

---

### D. Indicators

- Same session works across devices
- No IP / device binding
- No re-authentication required

---

## 2. Session Fixation

### Concept
Force victim to use a session ID you already know

---

### A. Attack flow

1. Attacker gets session ID:

```http
session=attacker123
```

2. Attacker sends link to victim:

```
http://target.com/login?session=attacker123
```

3. Victim logs in

4. Session remains:

```http
session=attacker123
```

5. Attacker uses same session → gains access

---

### B. Testing

1. Obtain session before login
2. Login
3. Check if session ID changes

→ If not → vulnerable

---

### C. Cookie-based fixation

Set cookie manually:

```http
Cookie: session=attacker123
```

Then login → does it persist?

---

## 3. Common Vulnerabilities

- No session regeneration after login
- Session ID accepted from URL
- Session not tied to user context
- Long-lived sessions

---

## 4. Additional Weaknesses

### A. No session binding

- Session works from:
  - different IP
  - different browser

---

### B. No session invalidation

- Logout does not destroy session

---

### C. Predictable session IDs

```text
session=1001 → 1002 → 1003
```

---

## 5. Indicators of Vulnerability

- Session ID unchanged after login
- Session reusable across devices
- Old sessions still valid
- Session accepted via URL

---

## 6. Fast Workflow (Exam Mode)

### Hijacking

1. Capture session cookie
2. Replay in another browser
3. Check access

---

### Fixation

1. Get session before login
2. Login
3. Compare session ID:
   - changed? OK
   - same? vulnerable

---

### Additional checks

- Logout → reuse session
- Change IP → reuse session

---

## 7. Mental Model

Session = identity

Hijacking:
→ Steal it

Fixation:
→ Predefine it

If session control is weak:
→ Password becomes irrelevant