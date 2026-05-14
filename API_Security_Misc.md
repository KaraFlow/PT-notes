## API — Security Misconfiguration

## Objective
Identify insecure configurations in APIs that expose:
- Sensitive data
- Internal functionality
- Weak security controls

Think: *“What did they forget to lock down?”*

---

## 1. Common Misconfigurations

### A. Debug / Verbose Errors Enabled

Look for:
- Stack traces
- File paths
- SQL errors
- Framework details

Example:
```json
{
  "error": "SQL syntax error near 'SELECT * FROM users'",
  "trace": "/var/www/app/db.js:45"
}
```

---

### B. Sensitive Information Exposure

Check responses for:
- API keys
- Tokens
- Internal IPs
- Emails / usernames
- Version info

---

### C. Default / Test Endpoints Enabled

Try:
```
/test
/debug
/dev
/status
/health
/metrics
```

---

### D. Directory / Endpoint Listing

```bash
GET /api/
GET /api/v1/
```

→ Returns list of endpoints

---

### E. Improper HTTP Methods Enabled

Test:

```bash
OPTIONS /api/resource
```

Look for:
```
Allow: GET, POST, PUT, DELETE
```

Try unexpected methods:
```bash
PUT
DELETE
PATCH
```

---

### F. CORS Misconfiguration

Check headers:

```http
Access-Control-Allow-Origin: *
Access-Control-Allow-Credentials: true
```

→ Dangerous combo

---

### G. Missing Security Headers

Check for absence of:

```http
X-Content-Type-Options
X-Frame-Options
Content-Security-Policy
Strict-Transport-Security
```

---

## 2. Where to Test

- All API responses
- Error responses
- Edge cases (invalid input)
- Preflight requests (CORS)

---

## 3. Testing Techniques

### A. Trigger errors intentionally

```json
{"id": "'"}
{"id": null}
{"id": []}
```

→ Look for verbose output

---

### B. Send unexpected methods

```bash
curl -X PUT http://target/api/user
curl -X DELETE http://target/api/user/1
```

---

### C. Check OPTIONS

```bash
curl -X OPTIONS http://target/api/user -i
```

---

### D. Inspect headers

```bash
curl -i http://target/api/user
```

---

### E. CORS testing

```http
Origin: evil.com
```

→ Does server allow it?

---

## 4. Dangerous Configurations

### Overly permissive CORS

```http
Access-Control-Allow-Origin: *
Access-Control-Allow-Credentials: true
```

---

### Exposed internal services

```json
{
  "db_host": "10.0.0.5",
  "redis": "localhost:6379"
}
```

---

### Debug mode enabled

Framework leaks:
- Express
- Django
- Spring Boot

---

### Backup / config files accessible

Try:
```
/.env
/config.json
/backup.zip
```

---

## 5. Indicators of Vulnerability

- Detailed error messages
- Internal paths or IPs
- Unexpected endpoints accessible
- Weak or missing headers
- Broad CORS permissions

---

## 6. Fast Workflow (Exam Mode)

1. Send normal request → inspect response
2. Trigger errors → look for leaks
3. Check headers:
   - security headers
   - CORS
4. Test methods:
   - OPTIONS
   - PUT / DELETE
5. Probe common endpoints:
   - /debug /test /status
6. Look for exposed config/data

---

## 7. Mental Model

Misconfigurations are not “bugs” — they’re oversights.

- Defaults left enabled
- Debug left on
- Access too broad

Your job:
→ Break normal flow → Observe what leaks