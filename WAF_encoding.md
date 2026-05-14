## Filter Evasion & WAF Bypass — Encoding Techniques

## Objective
Bypass filters and WAFs by encoding payloads so they:
- Evade detection
- Still execute after decoding

Think: *“Can I hide the payload but keep its effect?”*

---

## 1. HTML Encoding

### Concept
Encode characters using HTML entities

---

### Common encodings

```text
< → &lt;
> → &gt;
" → &quot;
' → &#39;
```

---

### Example

```html
<script>alert(1)</script>
→
&lt;script&gt;alert(1)&lt;/script&gt;
```

---

### Numeric encoding

```html
< → &#60;
> → &#62;
```

---

### Use case

- Bypass filters blocking `<script>`
- XSS payload obfuscation

---

## 2. URL Encoding

### Concept
Encode characters using `%` hex format

---

### Common examples

```text
< → %3C
> → %3E
" → %22
' → %27
/ → %2F
```

---

### Example

```text
<script>alert(1)</script>
→
%3Cscript%3Ealert(1)%3C/script%3E
```

---

### Double encoding

```text
< → %3C → %253C
```

---

### Use case

- Bypass input filters
- Encode payload in query params

---

## 3. Base64 Encoding

### Concept
Encode payload into Base64 string

---

### Example

```text
admin:password → YWRtaW46cGFzc3dvcmQ=
```

---

### Payload example

```text
<script>alert(1)</script>
→
PHNjcmlwdD5hbGVydCgxKTwvc2NyaXB0Pg==
```

---

### Use case

- Bypass filters expecting readable input
- Used in:
  - tokens
  - cookies
  - headers

---

## 4. Where Encoding Helps

- Input filters (blacklists)
- WAF rules
- Parameter validation
- Logging evasion

---

## 5. Combining Encodings

### Example

```text
<script>
→ URL + HTML encoded
%26lt%3Bscript%26gt%3B
```

---

### Layering

- URL encode → Base64 encode
- HTML encode → URL encode

---

## 6. Indicators of Success

- Payload passes filter
- Application decodes automatically
- Execution still occurs

---

## 7. Fast Workflow (Exam Mode)

1. Payload blocked?
2. Try:
   - URL encoding
   - HTML encoding
   - Base64 encoding
3. Try double encoding
4. Combine encodings
5. Observe response / execution

---

## 8. Mental Model

Filters match patterns.

Encoding breaks patterns.

But backend decodes.

Your job:
→ Hide payload → Let server reveal it