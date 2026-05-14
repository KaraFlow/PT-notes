## JavaScript for Pentesters — Multi-Level Data Parsing

## Objective
Extract data from complex/nested responses:
- HTML inside HTML
- JSON inside JSON
- XML inside XML

Think: *“Is the data buried inside another layer?”*

---

## 1. Multi-Level HTML Parsing

### Concept
HTML response contains embedded HTML

---

### Step 1: Parse main HTML

```javascript
var parser = new DOMParser();
var doc = parser.parseFromString(response, "text/html");
```

---

### Step 2: Extract inner HTML

```javascript
var inner = doc.querySelector('#content').innerHTML;
```

---

### Step 3: Parse again

```javascript
var doc2 = parser.parseFromString(inner, "text/html");
```

---

### Step 4: Extract target data

```javascript
var data = doc2.querySelector('.secret').innerText;
```

---

## 2. Multi-Level JSON Parsing

### Concept
JSON contains stringified JSON

---

### Example response

```json
{
  "data": "{\"user\":\"admin\",\"role\":\"admin\"}"
}
```

---

### Step 1: Parse outer JSON

```javascript
var obj = JSON.parse(response);
```

---

### Step 2: Parse inner JSON

```javascript
var inner = JSON.parse(obj.data);
```

---

### Step 3: Access data

```javascript
console.log(inner.user);
```

---

## 3. Multi-Level XML Parsing

### Concept
XML contains embedded XML

---

### Step 1: Parse XML

```javascript
var parser = new DOMParser();
var xml = parser.parseFromString(response, "application/xml");
```

---

### Step 2: Extract inner XML

```javascript
var inner = xml.querySelector('data').textContent;
```

---

### Step 3: Parse again

```javascript
var xml2 = parser.parseFromString(inner, "application/xml");
```

---

### Step 4: Extract data

```javascript
var value = xml2.querySelector('user').textContent;
```

---

## 4. Common Use Cases

- API responses with nested data
- Encoded or escaped payloads
- Data hidden in layers
- Bypassing filtering

---

## 5. Indicators of Multi-Level Data

- Escaped characters (`\"`, `&lt;`)
- JSON inside strings
- HTML inside elements
- XML text containing tags

---

## 6. Fast Workflow (Exam Mode)

1. Inspect response
2. Identify nested structure
3. Parse first layer
4. Extract inner data
5. Parse again
6. Repeat if needed
7. Extract final value

---

## 7. Mental Model

Data is often wrapped multiple times.

You don’t extract directly.

You:
→ Parse → Extract → Parse again  

Repeat until you reach:
→ The actual target data