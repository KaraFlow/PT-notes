## Command & Control (C2) — Deployment & Operation

## Objective
Understand how to:
- Deploy C2 infrastructure
- Establish initial access
- Maintain and operate control over targets

Think: *“How do I go from exploit → stable control?”*

---

## 1. Deployment Overview

```
1. Setup C2 server
2. Configure listener
3. Generate payload
4. Deliver payload
5. Receive callback
6. Operate session
```

---

## 2. C2 Server Setup

- VPS / cloud server
- Public IP or domain
- Open required ports (80 / 443 common)

---

### Considerations

- Use HTTPS for stealth
- Domain looks legitimate
- Avoid obvious indicators

---

## 3. Listener Configuration

- Defines how agents connect

---

### Example

- Protocol: HTTP/HTTPS
- Port: 443
- Endpoint: `/api/update`

---

### Goal

- Blend into normal traffic

---

## 4. Payload Generation

### Types

- Staged → small loader → fetch full agent
- Stageless → full payload upfront

---

### Considerations

- Encoding / obfuscation
- AV/EDR evasion
- Size vs stealth

---

## 5. Payload Delivery

### Common Methods

- Exploits (RCE, upload)
- Phishing
- File upload vulnerabilities
- Command injection

---

### Goal

- Execute payload on target

---

## 6. Callback / Initial Access

- Agent connects to listener

```http
POST /api/update
```

---

### Result

- New session appears in C2

---

## 7. Session Management

### Actions

- Interact with shell
- Run commands
- Upload/download files

---

### Stability

- Monitor connection
- Avoid killing session

---

## 8. Post-Exploitation

- Privilege escalation
- Persistence setup
- Credential harvesting
- Lateral movement

---

## 9. Persistence

### Techniques

- Startup scripts
- Scheduled tasks
- Service installation

---

### Goal

- Maintain access after reboot

---

## 10. Operational Security (OpSec)

- Limit noise (avoid excessive commands)
- Use jitter / delays
- Rotate infrastructure if needed
- Clean artifacts

---

## 11. Indicators of Issues

- No callback → delivery failed
- Session drops → unstable payload
- Detection alerts → poor evasion
- Blocked traffic → wrong protocol

---

## 12. Fast Workflow (Exam Mode)

1. Setup listener (HTTP/HTTPS)
2. Generate payload
3. Deliver via vulnerability
4. Wait for callback
5. Interact with session
6. Perform basic post-exploitation
7. Maintain access

---

## 13. Mental Model

C2 is a pipeline:

Exploit → Payload → Callback → Control  

If one step fails:
→ No control  

Your job:
→ Make the chain stable and stealthy