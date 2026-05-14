## Command & Control (C2) — How C2 Frameworks Work

## Objective
Understand how C2 frameworks operate to:
- Maintain remote access
- Execute commands
- Control compromised systems

Think: *“How does an attacker control a system after exploitation?”*

---

## 1. Core Concept

C2 = communication channel between:
- Attacker (controller)
- Compromised system (agent/beacon)

---

## 2. Basic Architecture

```
[ Attacker (C2 Server) ]
            ↑ ↓
     [ Agent / Beacon ]
            ↓
   [ Compromised Target ]
```

---

## 3. Key Components

### A. C2 Server
- Receives connections from targets
- Sends commands
- Collects data

---

### B. Agent / Beacon
- Runs on target
- Calls back to C2 server
- Executes commands

---

### C. Communication Channel
- HTTP / HTTPS (most common)
- DNS
- TCP / custom protocols

---

## 4. Communication Models

### A. Beaconing (Pull)

- Agent periodically checks in:

```
GET /tasks
```

→ C2 responds with commands

---

### B. Push Model

- C2 sends commands directly
- Requires open connection

---

## 5. Typical Workflow

1. Exploit target
2. Deploy agent (payload)
3. Agent connects to C2
4. Attacker sends commands
5. Agent executes and returns output

---

## 6. Common Capabilities

- Command execution
- File upload/download
- Screenshot / keylogging
- Persistence
- Lateral movement

---

## 7. Communication Characteristics

- Regular intervals (beaconing)
- Encrypted traffic (HTTPS)
- Mimics normal traffic
- Low noise (stealth)

---

## 8. Evasion Techniques

- Randomized intervals
- Domain fronting
- Legitimate-looking traffic
- Encoding/encryption

---

## 9. Indicators of C2 Activity

- Repeated outbound connections
- Unknown domains/IPs
- Regular timing patterns
- Suspicious HTTP requests

---

## 10. Mental Model

C2 is not just a connection.

It is:
→ Persistent control channel  
→ Hidden inside normal traffic  

Attacker goal:
→ Stay connected → Stay unnoticed → Execute remotely