# Backend Interview Preparation – Rohit Gadade

This document tracks the preparation steps, project architecture, and learning goals for a 2–3 month backend engineer switch plan.

---

## Goal
Become confident in:
- Backend architecture thinking
- System design discussions
- .NET internals
- Database engineering
- Azure usage (practical)

Build one production‑grade project:
**Smart Monitoring System**

---

## Project Architecture (Clean Architecture)

Layers:

### Domain
Contains pure business rules.
No dependency on database, frameworks, API, or cloud.

Example:
Device becomes offline if heartbeat not received for 2 minutes.

### Application
Contains use‑cases (actions system can perform).
Coordinates domain and defines interfaces.

Example:
RegisterDevice, SendHeartbeat, GenerateAlert

### Infrastructure
Implements database, cache, queues, external services.

Example:
SQL Server, Redis, RabbitMQ

### API
Receives HTTP requests and calls application layer.
No business logic.

Dependency Rule:
API → Application → Domain
Infrastructure → Application → Domain
Domain depends on nothing.

---

## Implemented So Far

### Domain Entity
Device entity with behavior:
- ReceiveHeartbeat()
- IsOffline(threshold)

Concept learned:
Entities protect invariants using private setters.

### Repository Contract
IDeviceRepository defined in Application.
Infrastructure implements it using EF Core.

Concept learned:
Dependency Inversion Principle.

### Use Case
RegisterDeviceCommand + Handler

Flow:
Controller → Command → Handler → Domain → Repository → Database

Concept learned:
CQRS style and vertical slice architecture.

---

## Database Setup
Using SQL Server via EF Core DbContext.
Tables created via migrations.

---

## Study Plan

### Phase 1 (Weeks 1–3)
C# internals + .NET architecture + database engineering

### Phase 2 (Weeks 4–7)
Caching, Azure, queues, background jobs, real‑time systems

### Phase 3 (Weeks 8–10)
System design + DSA + mock interviews

---

## Daily Study Method
3 hours daily:
1 hr concepts
1 hr coding
1 hr interview explanation practice

Sunday:
Revision + system design discussion

---

## Objective of Notes
These notes are written in explanation format so answers can be spoken confidently in interviews rather than memorized.