## Session Management Testing — Cross-Site Request Forgery (CSRF)

## Objective
Force a victim’s browser to perform actions on their behalf using their authenticated session.

Think: *“Can I make the user unknowingly send a valid request?”*

---

## 1. What is CSRF

- Victim is authenticated
- Attacker tricks victim into sending a request
- Browser automatically includes:
  - cookies
  - session tokens

---

## 2. Basic Attack Flow

1. Victim logs into target
2. Attacker sends malicious link/page
3. Victim visits it
4. Browser sends request with session
5. Action executed as victim

---

## 3. Where to Look

Sensitive actions:

- Change password
- Update email
- Transfer money
- Delete account
- Add admin user

---

## 4. Vulnerable Request Example

```http
POST /api/changeEmail
Cookie: session=abc123

email=attacker@test.com
```

→ No CSRF protection

---

## 5. Basic Exploit (GET)

```html
<img src="http://target.com/api/deleteUser?id=1">
```

---

## 6. POST Exploit

```html
<form action="http://target.com/api/changeEmail" method="POST">
  <input type="hidden" name="email" value="attacker@test.com">
</form>

<script>
document.forms[0].submit();
</script>
```

---

## 7. CSRF Protections to Check

### A. CSRF Token

```html
<input name="csrf_token" value="abc123">
```

→ Must be:
- unique per session/request
- validated server-side

---

### B. SameSite Cookie

```http
SameSite=Strict
SameSite=Lax
```

→ Restricts cross-site requests

---

### C. Referer / Origin Check

```http
Origin: https://trusted.com
```

→ Server validates source

---

## 8. Bypass Techniques

### A. Missing token

- No CSRF token → vulnerable

---

### B. Token not validated

- Token present but ignored

---

### C. Token predictable

```text
token=12345
```

---

### D. Token not tied to session

- Reuse token across users

---

### E. SameSite bypass

- `SameSite=None`
- `SameSite=Lax` (GET requests allowed)

---

### F. Method bypass

```http
POST → GET
```

---

## 9. Indicators of Vulnerability

- Action works without CSRF token
- Token reusable or predictable
- Request succeeds from external site
- No Origin/Referer validation

---

## 10. Testing Approach

1. Identify sensitive action
2. Capture request (Burp)
3. Remove CSRF token
4. Replay request
5. Try from external context (HTML page)
6. Observe:
   - action executed?
   - blocked?

---

## 11. Fast Workflow (Exam Mode)

1. Find state-changing request
2. Check for CSRF token
3. Remove/modify token
4. Replay request
5. Build PoC (HTML form/image)
6. Confirm execution

---

## 12. Mental Model

Browser sends credentials automatically.

Server must verify intent.

If not:
→ You control the user’s actions  
→ Without their awareness