## Server-Side — Insecure Deserialization (Java & PHP)

## Objective
Exploit unsafe deserialization to:
- Achieve RCE
- Manipulate application logic
- Bypass security controls

Think: *“Can I control what the server rebuilds into objects?”*

---

## 1. What is Deserialization

- App receives serialized data
- Converts it back into objects

If input is not validated → attacker controls object behavior

---

## 2. Where to Look

- Cookies
- POST data
- API parameters
- Hidden fields

---

### Common indicators

#### Java
- Base64 strings
- Binary blobs
- `rO0AB` (Java serialization header)

---

#### PHP
- Serialized strings:

```php
O:8:"ClassName":1:{s:4:"prop";s:5:"value";}
```

---

## 3. Java Insecure Deserialization

### Detection

Look for:

```text
rO0AB...
```

→ Base64 encoded Java object

---

### Basic test

1. Decode Base64
2. Modify object (if possible)
3. Re-encode
4. Send back

---

### Exploitation

Use tools:

- ysoserial

```bash
java -jar ysoserial.jar CommonsCollections1 "id" | base64
```

→ Inject payload into request

---

### Common sinks

- `readObject()`
- ObjectInputStream
- Custom deserialization logic

---

### Goal

Trigger gadget chain → execute command

---

## 4. PHP Insecure Deserialization

### Detection

Serialized format:

```php
O:4:"User":1:{s:5:"admin";b:1;}
```

---

### Modify values

```php
b:0 → b:1
```

→ Flip boolean

---

### Property manipulation

```php
s:5:"admin";b:0;
→
s:5:"admin";b:1;
```

---

### Exploitation vectors

- Magic methods:
  - `__wakeup()`
  - `__destruct()`
  - `__toString()`

→ Trigger code execution

---

### Example payload

```php
O:4:"User":2:{
  s:4:"name";s:5:"admin";
  s:5:"admin";b:1;
}
```

---

## 5. Common Exploitation Goals

- Set admin = true
- Inject file paths
- Trigger system calls
- Load malicious classes

---

## 6. Indicators of Vulnerability

- Serialized data in requests/cookies
- Base64 blobs decoded to objects
- Errors mentioning deserialization
- Unexpected behavior after modification

---

## 7. Bypass Techniques

- Recalculate string lengths (PHP)
- Maintain valid structure
- Encode payload (Base64 / URL)

---

## 8. Fast Workflow (Exam Mode)

1. Identify serialized input:
   - Base64
   - PHP object format
2. Decode
3. Modify:
   - flags (admin)
   - properties
4. Re-encode
5. Replay request
6. Observe:
   - privilege change
   - errors
   - execution

---

### If Java:

1. Use ysoserial
2. Generate payload
3. Inject into request
4. Look for execution

---

## 9. Mental Model

Server trusts object structure.

You control the object.

So you control:
→ Logic  
→ State  
→ Execution (if gadget chain exists)