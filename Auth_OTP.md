## Authentication Testing — Attacking Login Forms with OTP Security

## Objective
Exploit OTP-based authentication to:
- Bypass login
- Brute-force OTP codes
- Abuse logic flaws in verification

Think: *“Is the OTP actually protecting the login—or just added on top?”*

---

## 1. OTP Login Flow

```
1. User enters identifier (email/phone)
2. Server sends OTP
3. User submits OTP
4. Access granted
```

---

## 2. Common Weak Points

- OTP not tied to session/user
- Weak rate limiting
- Reusable OTPs
- Predictable OTPs
- Logic flaws in verification

---

## 3. OTP Brute-Force

### A. Small code space

```text
000000 → 999999
```

---

### B. No rate limiting

- Unlimited attempts allowed

---

### C. Automation

Use Burp Intruder:

- Payload: numeric range
- Monitor:
  - response length
  - status code

---

## 4. OTP Reuse

### Test:

- Use same OTP multiple times

→ Still valid?

---

### Indicators

- OTP not invalidated after use

---

## 5. OTP Expiration Issues

### A. Long validity

- OTP valid for minutes/hours

---

### B. No expiration

- Old OTP still works

---

## 6. OTP Not Bound to User

### Attack

1. Request OTP for user A
2. Use same OTP for user B

---

### Indicators

- OTP works across accounts

---

## 7. OTP Not Bound to Session

### Attack

1. Request OTP
2. Use it from different device/session

---

## 8. Parameter Manipulation

### Modify request

```json
{"otp":"123456"}
```

Try:

```json
{"otp":""}
{"otp":null}
```

---

### Add flags

```json
{"otp_verified": true}
```

---

## 9. Bypass via Direct API Access

- Skip frontend flow
- Call verification endpoint directly

---

## 10. Race Conditions

- Send multiple OTP submissions at once
- One may succeed without proper validation

---

## 11. OTP Disclosure

### Check responses

- OTP returned in API response
- OTP visible in logs / debug

---

## 12. Resend Abuse

- Request OTP repeatedly
- Flood system
- Predict patterns

---

## 13. Indicators of Vulnerability

- Unlimited OTP attempts
- OTP reusable
- OTP works across users/sessions
- No expiration enforcement
- OTP exposed in response

---

## 14. Fast Workflow (Exam Mode)

1. Trigger OTP
2. Test:
   - reuse OTP
   - expired OTP
3. Try brute-force:
   - multiple attempts
4. Modify request:
   - remove/alter OTP
5. Test cross-user/session usage
6. Check API responses for leaks
7. Confirm:
   - bypass or weakness

---

## 15. Mental Model

OTP should be:
- Unique
- Short-lived
- Single-use
- Bound to user/session

If not:
→ It becomes guessable or reusable  
→ And authentication weakens significantly