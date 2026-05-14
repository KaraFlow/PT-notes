## Authentication Testing — Weak Password & Lockout Mechanisms

## Objective
Identify weaknesses that allow:
- Easy password guessing
- Unlimited login attempts
- Bypass of protections (CAPTCHA / lockout)

Think: *“What stops me from guessing passwords—and can I bypass it?”*

---

## 1. Testing for Weak Password Policy

### A. Registration Testing

Try weak passwords:

```text
123456
password
admin123
test
```

---

### B. Policy checks

- Minimum length?
- Complexity enforced?
- Common passwords blocked?
- Reuse allowed?

---

### C. Bypass tricks

```text
password → password1
Password → P@ssword
```

→ Simple variations accepted?

---

### D. API-based registration

Send directly:

```json
{"username":"test","password":"123"}
```

→ Bypass frontend validation

---

### Indicators

- Weak passwords accepted
- No complexity enforcement
- No blacklist of common passwords

---

## 2. Weak Lockout Mechanism — CAPTCHA Bypass

### Objective
Bypass CAPTCHA to allow automated attacks

---

### A. Skip CAPTCHA parameter

Remove it:

```json
{"username":"admin","password":"test"}
```

→ Does request still process?

---

### B. Reuse CAPTCHA

- Solve once
- Replay same value

---

### C. Modify CAPTCHA field

```json
{"captcha":"1234"}
```

→ Is it validated?

---

### D. Direct API calls

- CAPTCHA often enforced only in frontend
- Send request via Burp/curl

---

### E. Race condition

- Send multiple requests before CAPTCHA validated

---

### Indicators

- Login works without CAPTCHA
- Same CAPTCHA reused
- No validation server-side

---

## 3. Weak Lockout Mechanism — Account Lockout Bypass

### A. No lockout

- Unlimited login attempts allowed

---

### B. Weak lockout threshold

- Lockout after many attempts (e.g. 100+)

---

### C. Reset attempts via timing

- Wait a few seconds → counter resets

---

### D. Username variation

```text
admin
Admin
ADMIN
```

→ Different counters?

---

### E. IP-based bypass

- Change IP (VPN / headers)

```http
X-Forwarded-For: 1.2.3.4
```

---

### F. Distributed attempts

- Spread attempts across:
  - multiple IPs
  - multiple accounts

---

### G. Partial lockout

- Only UI blocked
- API still accessible

---

## 4. Indicators of Weak Protection

- No rate limiting
- No lockout after multiple attempts
- CAPTCHA easily bypassed
- Different behavior between UI and API

---

## 5. Tools / Approach

- Burp Intruder (brute-force)
- Wordlists (common passwords)
- Monitor:
  - response codes
  - timing
  - lockout messages

---

## 6. Fast Workflow (Exam Mode)

1. Test registration:
   - weak passwords
2. Test login brute-force:
   - multiple attempts
3. Check CAPTCHA:
   - remove / reuse / modify
4. Test lockout:
   - number of attempts
   - timing reset
5. Try bypass:
   - change IP
   - vary username
6. Confirm:
   - unlimited attempts?
   - weak protection?

---

## 7. Mental Model

Protections are often:
- Frontend only
- Poorly enforced
- Inconsistent

Your job:
→ Remove the guardrails → Automate attacks