## API — Parameter Tampering

## Objective
Manipulate parameters to:
- Access unauthorized data
- Modify application behavior
- Escalate privileges
- Bypass business logic

Think: *“What happens if I change what the client is supposed to control?”*

---

## 1. What to Target

Parameters that influence:
- Identity (user_id, account_id)
- Roles / permissions
- Prices / quantities
- Status / flags
- Workflow steps

---

## 2. Where to Tamper

- Query params
```
/api/order?id=1001
```

- JSON body
```json
{"user_id": 1}
```

- Headers
```
X-User-Id: 1
X-Role: user
```

- Hidden / optional fields

---

## 3. ID Tampering (High Yield)

### Change identifiers

```http
GET /api/user/1 → /api/user/2
```

```json
{"user_id": 1} → {"user_id": 999}
```

---

### Common fields

```json
"id"
"user_id"
"account_id"
"order_id"
"invoice_id"
```

---

## 4. Role / Permission Tampering

### Modify role fields

```json
{"role": "user"} → {"role": "admin"}
{"isAdmin": false} → {"isAdmin": true}
```

---

### Hidden flags

```json
{"verified": false} → {"verified": true}
{"status": "pending"} → {"status": "approved"}
```

---

## 5. Price / Logic Manipulation

### Change price / quantity

```json
{"price": 100} → {"price": 1}
{"quantity": 1} → {"quantity": 100}
```

---

### Discount abuse

```json
{"discount": 0} → {"discount": 100}
```

---

## 6. Workflow / State Tampering

### Skip steps

```json
{"step": 1} → {"step": 5}
```

---

### Force final state

```json
{"status": "pending"} → {"status": "completed"}
```

---

## 7. Hidden Parameter Discovery

### Add unexpected fields

```json
{
  "username": "user",
  "isAdmin": true
}
```

---

### Use Burp to find parameters

- Proxy → capture request
- Repeater → add/remove fields
- Intruder → fuzz names

---

## 8. Parameter Pollution

### Duplicate params

```http
GET /api/user?id=1&id=2
```

---

### JSON duplication

```json
{"id":1,"id":2}
```

→ Backend may pick first/last

---

## 9. Header Tampering

### Modify trust-based headers

```http
X-Forwarded-For: 127.0.0.1
X-User-Role: admin
X-Admin: true
```

---

## 10. Encoding / Format Tricks

```json
{"role": "admin "}
{"role": "ADMIN"}
{"role": "admin\n"}
```

---

## 11. Common Vulnerabilities

- Trusting client-side input
- Missing server-side validation
- No ownership checks
- Business logic flaws
- Mass assignment

---

## 12. Indicators of Vulnerability

- Behavior changes after parameter modification
- Access to other users’ data
- Privileged actions succeed
- No validation errors

---

## 13. Fast Workflow (Exam Mode)

1. Capture request (Burp)
2. Identify key parameters:
   - IDs
   - roles
   - flags
3. Modify values:
   - increment IDs
   - set admin flags
   - change logic fields
4. Add new parameters
5. Try duplicates
6. Observe response differences

---

## 14. Mental Model

APIs often trust:
- What the client sends
- Hidden fields
- “Unchangeable” values

They shouldn’t.

Your job:
→ Change the input → Break the logic → Gain control