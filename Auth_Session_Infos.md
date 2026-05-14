## Authentication & Session Management — Fundamentals

## Objective
Understand how authentication and sessions work to:
- Identify weaknesses
- Target the right components

Think: *“How does the app know who I am, and how does it keep track of it?”*

---

## 1. Types of Authentication Mechanisms

### A. Basic Authentication

```http
Authorization: Basic base64(username:password)
```

- Simple, stateless
- Credentials sent every request

---

### B. Token-Based Authentication

```http
Authorization: Bearer <token>
```

- JWT or random tokens
- Used in APIs

---

### C. Session-Based Authentication

- Server creates session ID
- Stored in cookie:

```http
Cookie: PHPSESSID=abc123
```

---

### D. API Keys

```http
X-API-Key: abc123
```

- Static key for access
- Often weak if exposed

---

### E. OAuth / SSO

- Third-party authentication (Google, etc.)
- Uses tokens and redirects

---

## 2. What is Session Management

### Concept

After login:
- Server creates session
- Client stores session ID
- Session ID used to identify user

---

### Flow

1. User logs in
2. Server validates credentials
3. Server generates session/token
4. Client stores it (cookie/header)
5. Sent with every request

---

## 3. Session Storage

### Cookies (most common)

```http
Set-Cookie: session=abc123
```

---

### Headers (APIs)

```http
Authorization: Bearer token
```

---

### Local storage (frontend)

- Stored in browser (less secure)

---

## 4. Key Concepts

### Authentication
→ Who are you?

---

### Authorization
→ What can you do?

---

### Session
→ How the server remembers you

---

## 5. Key Risks

- Weak authentication methods
- Predictable or reusable tokens
- Poor session handling
- Missing expiration
- Insecure storage

---

## 6. Where to Focus (Testing Mindset)

- Login mechanism
- Token generation
- Session handling
- Token reuse / expiration
- Access control tied to session

---

## 7. Mental Model

Authentication = identity check  
Session = persistence of identity  

If you can:
→ Steal it  
→ Predict it  
→ Reuse it  

You become the user