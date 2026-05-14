## Authentication Testing — Attacking Two-Factor Authentication (2FA)

## Objective
Bypass or weaken 2FA protections to:
- Gain access without second factor
- Reduce 2FA effectiveness
- Take over accounts

Think: *“Is 2FA really enforced—or just cosmetic?”*

---

## 1. 2FA Flow (Basic)

```
1. User enters username/password
2. Server validates credentials
3. Server asks for 2FA code
4. User submits code
5. Access granted
```

---

## 2. Common Weak Points

- 2FA not enforced everywhere
- Logic flaws in verification step
- Weak or reusable codes
- Bypass via direct API access

---

## 3. Bypassing 2FA

### A. Skip 2FA Step

After login, try accessing protected endpoints directly:

```http
GET /api/dashboard
```

→ Without submitting 2FA

---

### B. Direct API access

- Complete login via API
- Skip frontend 2FA enforcement

---

### C. Manipulate verification flags

```json
{"2fa_verified": false} → {"2fa_verified": true}
```

---

### D. Remove 2FA parameter

```json
{"code": "123456"} → {}
```

→ Does it still validate?

---

## 4. Brute-Forcing 2FA Codes

### A. Weak rate limiting

- Try multiple codes:

```text
000000 → 999999
```

---

### B. No lockout

- Unlimited attempts allowed

---

### C. Small code space

- 4–6 digits → brute-force possible

---

## 5. Code Reuse / Expiration Issues

### A. Reuse valid code

- Use same code multiple times

---

### B. Expired codes still valid

- Wait → reuse code

---

### C. Long validity window

- Codes valid for too long

---

## 6. Backup Code Weaknesses

- Reusable backup codes
- Predictable codes
- No invalidation after use

---

## 7. 2FA Reset / Recovery Bypass

### A. Weak reset process

- Reset 2FA without proper verification

---

### B. Email-only reset

- No additional checks

---

### C. Parameter tampering

```json
{"2fa_enabled": true} → {"2fa_enabled": false}
```

---

## 8. Race Conditions

- Send multiple verification requests at once
- One succeeds without proper validation

---

## 9. Indicators of Vulnerability

- Access without completing 2FA
- Unlimited code attempts
- Code reuse works
- No expiration enforcement
- Weak reset process

---

## 10. Fast Workflow (Exam Mode)

1. Login with valid creds
2. Before 2FA:
   - access protected endpoints
3. Test verification:
   - remove/modify code
4. Try brute-force:
   - multiple attempts
5. Test reuse:
   - same code again
6. Check reset flow
7. Confirm:
   - bypass or weakness

---

## 11. Mental Model

2FA should be:
- Required
- Verified server-side
- Strictly enforced

If logic is weak:
→ It becomes optional  
→ And authentication collapses