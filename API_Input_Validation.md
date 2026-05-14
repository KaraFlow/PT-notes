## API — Improper Input Validation

## Objective
Identify weak or missing validation allowing:
- Unexpected data types
- Business logic bypass
- Injection opportunities
- Crashes / abnormal behavior

Think: *“What happens if I send something they didn’t expect?”*

---

## 1. What to Break

APIs often assume:
- Correct data types
- Valid formats
- Expected ranges

You test:
- Type confusion
- Missing fields
- Extra fields
- Malformed input

---

## 2. Where to Test

- Query parameters
- JSON body
- Headers
- Path parameters

Example:
```http
GET /api/user?id=1
```

```json
{"email": "user@test.com"}
```

---

## 3. Type Confusion (High Yield)

### Send wrong types

```json
{"id": "string"}
{"id": []}
{"id": {}}
{"id": true}
```

---

### Numeric edge cases

```json
{"id": -1}
{"id": 0}
{"id": 999999999999}
```

---

### Null / empty

```json
{"id": null}
{"id": ""}
```

---

## 4. Missing / Extra Parameters

### Remove required fields

```json
{}
```

→ Does it still process?

---

### Add unexpected fields

```json
{
  "username": "user",
  "isAdmin": true
}
```

→ Mass assignment risk

---

## 5. Format Validation

### Email / input formats

```json
{"email": "not-an-email"}
{"email": "a@a"}
{"email": "test@test.com<script>"}
```

---

### JSON structure abuse

```json
{"id": {"nested": "value"}}
{"id": [1,2,3]}
```

---

## 6. Length & Boundary Testing

### Long input

```json
{"name": "A"*10000}
```

---

### Short / empty

```json
{"name": ""}
```

---

### Special characters

```json
{"name": "!@#$%^&*()"}
```

---

## 7. Parameter Pollution

### Duplicate parameters

```http
GET /api/user?id=1&id=2
```

→ Which one is used?

---

### JSON duplication

```json
{"id":1,"id":2}
```

---

## 8. Encoding Tricks

```json
{"id": "1%20OR%201=1"}
{"id": "1\n"}
{"id": "1\t"}
```

---

## 9. Boolean / Logic Abuse

```json
{"active": "true"}
{"active": 1}
{"active": "yes"}
```

→ Does backend treat all as true?

---

## 10. Array Injection

```json
{"id": [1,2,3]}
```

→ Backend may process first value only

---

## 11. Common Vulnerabilities

- Type confusion → bypass checks
- Missing validation → unexpected execution
- Mass assignment
- Business logic flaws
- Injection (SQL / command)

---

## 12. Indicators of Vulnerability

- Server accepts invalid input
- Different behavior for same endpoint
- Crashes / 500 errors
- Unexpected data returned
- Silent acceptance of bad data

---

## 13. Fast Workflow (Exam Mode)

1. Identify input fields
2. For each field:
   - change type
   - send null / empty
   - send large values
3. Remove required fields
4. Add extra fields
5. Try duplicates / arrays
6. Observe:
   - response differences
   - errors
   - logic bypass

---

## 14. Mental Model

Validation is often:
- Done on frontend only
- Inconsistent across endpoints
- Weak for edge cases

Your job:
→ Break format → Break logic → Find bypass