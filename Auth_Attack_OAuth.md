## Authentication Testing — Attacking OAuth

## Objective
Exploit weaknesses in OAuth flows to:
- Take over accounts
- Steal tokens
- Bypass authentication

Think: *“Can I interfere with the trust between client, provider, and user?”*

---

## 1. OAuth Basics (Simplified Flow)

```
User → App → OAuth Provider → App → User logged in
```

---

### Key components

- Authorization endpoint
- Redirect URI
- Authorization code / token
- Access token

---

## 2. Common Attack Points

- `redirect_uri`
- `state`
- Authorization code
- Access token

---

## 3. Redirect URI Manipulation

### Objective
Redirect OAuth response to attacker-controlled endpoint

---

### Example

```
redirect_uri=https://attacker.com
```

---

### Attack

1. Modify `redirect_uri`
2. Victim authenticates
3. Token/code sent to attacker

---

### Weak validation cases

- Partial match
- Wildcards
- Subdomain tricks

---

### Bypass examples

```
https://trusted.com.attacker.com
https://trusted.com@attacker.com
```

---

## 4. Missing / Weak State Parameter

### Purpose

- Prevent CSRF in OAuth flow

---

### Attack

- Remove or reuse `state`
- Force victim to use attacker’s session

---

### Result

→ Account linked to attacker-controlled session

---

## 5. Authorization Code Interception

### Flow

1. Victim logs in
2. Receives `code`
3. App exchanges code for token

---

### Attack

- Intercept or reuse code
- Use it before victim

---

### Indicators

- Code reusable
- No binding to session/client

---

## 6. Token Leakage

### Where tokens leak

- URL parameters
- Referer headers
- Logs

---

### Example

```
https://app.com/callback#access_token=abc123
```

---

### Attack

- Capture token
- Replay request

---

## 7. Open Redirect in OAuth

### Combine with redirect_uri

```
redirect_uri=https://trusted.com/redirect?url=attacker.com
```

→ OAuth trusts trusted.com → redirects to attacker

---

## 8. Scope Manipulation

### Modify scope

```
scope=read → scope=read+write+admin
```

---

### Result

→ Excessive permissions granted

---

## 9. Common Vulnerabilities

- Weak redirect URI validation
- Missing/weak state parameter
- Code/token reuse
- Token exposure in URLs
- Overly permissive scopes

---

## 10. Indicators of Vulnerability

- Token/code received on attacker endpoint
- Login linked to wrong account
- Same code reused successfully
- Unexpected permissions granted

---

## 11. Fast Workflow (Exam Mode)

1. Capture OAuth request
2. Modify:
   - redirect_uri
   - state
   - scope
3. Observe flow
4. Try:
   - intercept code
   - reuse code
5. Check token exposure
6. Confirm:
   - account takeover
   - token leak

---

## 12. Mental Model

OAuth = trust chain

- App trusts provider
- Provider trusts redirect URI
- App trusts returned token

Break any link:
→ You control authentication