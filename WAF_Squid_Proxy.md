## Filter Evasion & WAF Bypass — Bypassing Squid Proxy (Browser-Based Restrictions)

## Objective
Bypass Squid proxy restrictions enforced at browser/network level to:
- Access blocked websites
- Reach restricted endpoints
- Evade filtering rules

Think: *“Is the restriction on the browser, DNS, or the proxy itself?”*

---

## 1. Understanding the Setup

Typical flow:

```
Browser → Squid Proxy → Internet
```

Restrictions may be based on:
- Domain names
- URLs / paths
- Keywords
- IP addresses

---

## 2. Common Restriction Types

- Blocked domains (e.g. example.com)
- Blocked keywords (e.g. "admin", "proxy")
- URL pattern filtering
- File type restrictions

---

## 3. Bypass Techniques

### A. Use IP instead of domain

```text
http://example.com → http://93.184.216.34
```

---

### B. Alternative domain formats

```text
http://example.com.
http://example.com:80
http://example.com@attacker.com
```

---

### C. URL encoding

```text
http://example.com/admin
→
http://example.com/%61dmin
```

---

### D. Case variation

```text
/admin → /AdMiN
```

---

### E. Fragment trick

```text
http://example.com#@blocked.com/
```

---

### F. Path confusion

```text
http://example.com/./admin
http://example.com//admin
```

---

### G. Parameter pollution

```text
http://example.com/admin?allowed=1
```

---

### H. Use HTTPS instead of HTTP

```text
http://blocked.com → https://blocked.com
```

→ Proxy may not inspect encrypted traffic

---

### I. DNS tricks

- Use alternative DNS resolver
- Use subdomains:

```text
allowed.com.blocked.com
```

---

### J. Open redirect abuse

1. Access allowed site:
```text
http://allowed.com/redirect?url=http://blocked.com
```

2. Proxy allows initial domain → redirects to blocked target

---

### K. Web-based proxies

Use allowed sites that fetch content:

```text
http://allowed.com/proxy?url=http://blocked.com
```

---

## 4. Indicators of Bypass

- Blocked site becomes accessible
- Different response behavior
- Proxy does not trigger block page

---

## 5. Common Weaknesses

- String-based filtering only
- No normalization (case, encoding)
- Domain-based checks only
- Weak URL parsing

---

## 6. Fast Workflow (Exam Mode)

1. Identify restriction type:
   - domain / keyword / path
2. Try:
   - IP instead of domain
   - encoding
   - case variation
3. Try URL tricks:
   - fragments
   - path confusion
4. Try HTTPS
5. Try redirect via allowed domain
6. Observe response differences

---

## 7. Mental Model

Proxy filters:
- Compare strings
- Often don’t normalize input

You win by:
→ Changing representation, not destination
→ Same target, different form