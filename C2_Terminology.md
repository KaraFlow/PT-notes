## Command & Control (C2) — Framework Terminology

## Objective
Understand key C2 terms to:
- Follow attack flow
- Interpret tools/output
- Communicate clearly in reports

Think: *“What does each component actually represent?”*

---

## 1. Core Terms

### C2 Server (Team Server)
- Central control system
- Sends commands / receives data

---

### Agent / Implant / Payload
- Code running on target
- Executes commands from C2

---

### Beacon
- Agent that periodically checks in
- Used in “pull” communication model

---

### Listener
- Interface on C2 server waiting for connections
- Example: HTTP listener on port 80/443

---

## 2. Communication Terms

### Callback
- Connection from agent → C2

---

### Check-in
- Periodic request from agent

```http
GET /tasks
```

---

### Tasking
- Commands sent to agent

---

### Output / Response
- Data returned from agent

---

## 3. Execution Terms

### Command Execution
- Running system commands on target

---

### Module
- Predefined functionality (e.g., dump creds)

---

### Job
- Background task executed on agent

---

## 4. Persistence Terms

### Persistence
- Mechanism to survive reboot

---

### Stager
- Small initial payload
- Downloads full agent

---

### Stage
- Full payload delivered after stager

---

## 5. Network & Stealth Terms

### Beacon Interval
- Time between callbacks

---

### Jitter
- Random delay added to avoid detection

---

### Domain Fronting
- Hiding traffic behind legitimate domains

---

### Encoding / Obfuscation
- Hiding payload or traffic content

---

## 6. Lateral Movement Terms

### Pivoting
- Using compromised host to reach others

---

### Tunnel
- Forwarding traffic through compromised system

---

## 7. Indicators (Useful for Testing)

- Regular beacon intervals
- Repeated callbacks
- Encoded or unusual traffic
- Known C2 patterns

---

## 8. Fast Mapping (Exam Mental Model)

| Term       | Meaning                     |
|------------|----------------------------|
| Agent      | Code on target             |
| Beacon     | Periodic callback agent    |
| Listener   | Entry point on C2 server   |
| Tasking    | Commands sent              |
| Callback   | Agent → C2 communication   |
| Stager     | Initial loader             |
| Stage      | Full payload               |

---

## 9. Mental Model

C2 language describes control flow:

- Agent connects → callback  
- C2 sends → tasking  
- Agent executes → returns output  

Everything revolves around:
→ Maintaining control over the target