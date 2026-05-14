## Authentication Testing — Username Enumeration

## Objective
Identify valid usernames by observing differences in responses during authentication.

Think: *“Can I tell if a user exists without knowing the password?”*

---

## 1. Why It Matters

- Confirms valid accounts
- Enables targeted brute-force attacks
- Reduces attack surface uncertainty

---

## 2. Where to Test

- Login forms
- Password reset
- Registration
- Account recovery
- API login endpoints

---

## 3. Detection Techniques

### A. Response Message Differences

```text
Invalid username
vs
Invalid password
```

→ Clear enumeration

---

### B. Generic vs Specific Errors

```text
Login failed
```

But:

- Different wording
- Different structure

---

### C. Response Time Differences

- Valid user → password check → slower
- Invalid user → immediate reject

---

### D. HTTP Status Codes

```text
401 vs 404 vs 200
```

---

### E. Response Length

Compare:

- Content length
- JSON structure

---

### F. Redirect Behavior

- Valid user → redirect to password step
- Invalid user → stays on login

---

## 4. Password Reset Enumeration

### Example

```text
"If email exists, we sent a reset link"
```

But:

- Valid → email sent (delay / different response)
- Invalid → instant response

---

## 5. API-Based Enumeration

### Example request

```json
{"username": "admin", "password": "test"}
```

Look for:

- Different error messages
- Different response codes
- Timing differences

---

## 6. Automation

### Burp Intruder

- Payload list: usernames
- Compare:
  - response length
  - status codes
  - response time

---

### Example wordlist

```text
admin
administrator
test
user
john
```

---

## 7. Indicators of Vulnerability

- Distinct error messages
- Timing discrepancies
- Response size differences
- Different status codes

---

## 8. Mitigations (what devs should do)

- Generic error messages
- Constant response time
- Same status codes
- Rate limiting

---

## 9. Fast Workflow (Exam Mode)

1. Identify login/reset endpoint
2. Send invalid username
3. Send valid-looking username
4. Compare:
   - message
   - time
   - status
   - length
5. Automate with wordlist
6. Confirm valid usernames

---

## 10. Mental Model

Login is a two-step check:

1. Does user exist?
2. Is password correct?

If responses differ:
→ You can separate the two  
→ And enumerate users