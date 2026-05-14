## Advanced Injection Attacks — Second-Order SQL Injection

## Objective
Exploit SQL injection that is:
- Stored first
- Executed later in a different context

Think: *“Can I plant payload now and trigger it later?”*

---

## 1. What is Second-Order SQLi

- Input is stored safely (no immediate execution)
- Later reused in SQL query without proper sanitization
- Injection triggers at second step

---

## 2. Attack Flow

```
1. Inject payload → stored in DB
2. Application uses stored value in query
3. Payload executed → SQLi triggered
```

---

## 3. Where to Look

- Registration forms
- Profile fields
- Comments / messages
- File names
- Logs (User-Agent, etc.)

---

## 4. Example Scenario

### Step 1: Inject payload

```text
username = admin'--
```

---

### Step 2: Application stores it

---

### Step 3: Later query

```sql
SELECT * FROM users WHERE username = '<stored_value>'
```

→ Becomes:

```sql
SELECT * FROM users WHERE username = 'admin'--'
```

→ Injection executed

---

## 5. Testing Strategy

### A. Inject payload into stored field

```text
' OR 1=1--
```

---

### B. Trigger functionality that uses it

- Login
- Search
- Admin panel
- Profile display

---

### C. Observe behavior

- Auth bypass
- Errors
- Data leakage

---

## 6. Blind Second-Order SQLi

If no direct output:

### Boolean-based

```text
' AND 1=1--
' AND 1=2--
```

---

### Time-based

```text
' OR SLEEP(5)--
```

---

→ Triggered when stored value is used

---

## 7. Common Vulnerabilities

- Input sanitized on insert but not on reuse
- Trusting stored data
- Dynamic query building with stored values

---

## 8. Indicators of Vulnerability

- No issue at input stage
- Issue appears later in different feature
- Errors or unexpected behavior when data reused

---

## 9. Tools / Approach

- Manual testing (very important)
- Burp Suite:
  - store payload
  - replay later request

---

## 10. Fast Workflow (Exam Mode)

1. Find input stored in DB
2. Inject payload:
   - ' OR 1=1--
3. Identify feature that uses stored data
4. Trigger it
5. Test:
   - boolean-based
   - time-based
6. Confirm execution

---

## 11. Mental Model

First request:
→ Nothing happens  

Second request:
→ Payload executes  

You are planting a delayed exploit.

Your job:
→ Inject now → Trigger later → Exploit