## Filter Evasion & WAF Bypass — Input Filtering

## Objective
Bypass input validation mechanisms on:
- Client-side (browser)
- Server-side (backend / WAF)

Think: *“What is being enforced, and where can I bypass it?”*

---

## 1. Bypassing Client-Side Filters

### Key Idea
Client-side validation is **not security**

→ You control the request

---

### A. Modify requests directly

Use Burp / curl instead of browser

```bash
curl -X POST http://target/api \
-d '{"role":"admin"}'
```

---

### B. Disable JavaScript

- Turn off JS in browser
- Submit form manually

---

### C. Edit request in browser

- DevTools → Network → Modify request
- Change values before sending

---

### D. Remove restrictions

HTML constraints:

```html
<input type="number" max="10">
```

→ Send:

```json
{"value": 999}
```

---

### E. Hidden fields

```html
<input type="hidden" name="role" value="user">
```

→ Modify:

```json
{"role": "admin"}
```

---

## 2. Bypassing Server-Side Filters

### A. Encoding bypass

```text
<script> → %3Cscript%3E
```

---

### B. Case variation

```text
SELECT → select → SeLeCt
```

---

### C. Whitespace tricks

```text
UNION SELECT
UNION/**/SELECT
```

---

### D. Alternate syntax

```text
OR 1=1
|| 1=1
```

---

### E. Partial filtering bypass

If only certain keywords blocked:

```text
UNION → UN/**/ION
```

---

### F. Parameter fragmentation

```http
GET /api?id=1 OR 1=1
→
GET /api?id=1&id=OR 1=1
```

---

### G. JSON tricks

```json
{"id": "1 OR 1=1"}
{"id": ["1 OR 1=1"]}
```

---

### H. Null byte / control chars

```text
admin%00
admin\n
admin\t
```

---

## 3. Weak Filter Patterns

### Blacklist filtering

Blocks:
```
<script>
```

But allows:
```
<ScRiPt>
%3Cscript%3E
```

---

### Partial sanitization

Removes:
```
'
```

But allows:
```
" OR 1=1
```

---

## 4. Indicators of Filtering

- Input modified in response
- Certain characters removed
- Different error messages
- Payload partially executed

---

## 5. Fast Workflow (Exam Mode)

1. Send payload → blocked?
2. Identify:
   - client-side or server-side?
3. If client-side:
   - bypass using Burp / curl
4. If server-side:
   - encode payload
   - change case
   - add comments / spaces
5. Try variants until it passes

---

## 6. Mental Model

Client-side = cosmetic  
Server-side = imperfect  

Filters:
- Look for patterns
- Rarely complete

Your job:
→ Change shape, not intent → Slip through