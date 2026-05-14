## Command & Control (C2) — Choosing the Correct C2 Framework

## Objective
Select the appropriate C2 framework based on:
- Target environment
- Engagement constraints
- Desired capabilities

Think: *“What do I need to achieve—and what constraints do I have?”*

---

## 1. Key Decision Factors

### A. Target Environment

- OS: Windows / Linux / mixed
- Network restrictions (egress filtering)
- AV / EDR presence

---

### B. Communication Constraints

- Allowed protocols:
  - HTTP / HTTPS (most common)
  - DNS
  - TCP

- Outbound-only vs inbound allowed

---

### C. Stealth Requirements

- Low noise vs fast exploitation
- Need for obfuscation
- Detection risk tolerance

---

### D. Engagement Scope

- Short-term access
- Long-term persistence
- Lateral movement required?

---

## 2. Framework Characteristics

### A. Lightweight C2

- Minimal features
- Fast setup
- Lower detection surface

→ Use when:
- Simple command execution needed
- Limited time

---

### B. Full-Featured C2

- Modular capabilities
- Persistence, pivoting, post-exploitation

→ Use when:
- Deep engagement
- Full control required

---

### C. Custom / Minimal Payloads

- Highly stealthy
- Limited functionality

→ Use when:
- Strict detection controls
- Red team scenarios

---

## 3. Communication Strategy

### HTTP/HTTPS

- Blends with normal traffic
- Most reliable

---

### DNS

- Useful for restricted environments
- Slower data transfer

---

### TCP / Custom

- More control
- Higher detection risk

---

## 4. Payload Considerations

- Staged vs stageless
- Size of payload
- Encoding / obfuscation
- AV evasion

---

## 5. Trade-Offs

| Factor        | Option A (Stealth) | Option B (Power) |
|---------------|------------------|------------------|
| Visibility    | Low              | Higher           |
| Features      | Limited          | Extensive        |
| Setup         | Simple           | Complex          |
| Detection     | Lower            | Higher           |

---

## 6. Practical Approach (Exam Mindset)

1. Identify target environment
2. Check network constraints
3. Choose communication channel:
   - HTTP → default
   - DNS → restricted env
4. Select payload type:
   - staged → flexibility
   - stageless → reliability
5. Balance:
   - stealth vs capability

---

## 7. Indicators of Poor Choice

- Payload blocked immediately
- No outbound communication
- High detection / alerts
- Unstable connection

---

## 8. Mental Model

There is no “best” C2.

Only:
→ Best fit for the situation  

You are balancing:
- Access
- Stealth
- Capability  

Pick what works—not what is powerful