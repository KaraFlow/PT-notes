## Server-Side — Modern Web Application Architecture

## Objective
Understand how modern apps are structured to:
- Map attack surface
- Identify trust boundaries
- Target the right components

---

## 1. Typical Architecture

Client → API Gateway → Backend Services → Database

```
[ Browser / Mobile ]
        ↓
   [ Frontend (React, etc.) ]
        ↓
      [ API ]
        ↓
 [ Microservices / Logic ]
        ↓
   [ Database / Storage ]
```

---

## 2. Key Components

### Frontend (Client)
- React / Vue / mobile apps
- Handles UI only
- **Not trusted**

---

### API Layer
- REST / GraphQL / SOAP
- Handles requests from client
- Main attack entry point

---

### Backend Services
- Business logic
- Auth checks
- Data processing

---

### Database
- Stores application data
- SQL / NoSQL

---

## 3. Common Patterns

### Monolith
- Single backend
- Easier to map

---

### Microservices
- Multiple services
- Internal APIs
- More attack surface

---

### API Gateway
- Central entry point
- Routes requests to services

---

## 4. Trust Boundaries

Critical zones:

- Client ↔ API
- API ↔ Backend services
- Backend ↔ Database

Never assume trust across boundaries

---

## 5. Data Flow

1. Client sends request
2. API validates input
3. Backend processes logic
4. Database queried
5. Response returned

---

## 6. Where Attacks Happen

- Input handling (API)
- Business logic (backend)
- Service-to-service communication
- Data access layer

---

## 7. Key Risks

- Over-trusting client input
- Weak internal API security
- Misconfigured services
- Lack of validation between components

---

## 8. Mental Model

Frontend is just a viewer.

Real control is:
→ API → Backend → Data

Your job:
→ Interact directly with API
→ Ignore frontend restrictions
→ Trace how input flows through the system

