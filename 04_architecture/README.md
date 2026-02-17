Clean Architecture Layers
  -Clean Architecture separates business logic from frameworks like database, UI, and external services so that core rules remain stable even if       technology changes.

Layers
Domain (Core Business Rules)
  -Contains enterprise rules and entities.
  -No dependency on database, API, or frameworks.
Example:
  -A device becomes offline if heartbeat not received for 2 minutes.

Application (Use Cases)
  -Contains system actions and workflows.
  -Coordinates domain entities and defines interfaces for repositories/services.

Example:
  -RegisterDevice, ProcessHeartbeat, GenerateAlert

Infrastructure (External Implementation)
  -Implements database, cache, messaging, email, or third-party integrations.

Example:
  -SQL Server repository, Redis cache, RabbitMQ publisher

API / Presentation
  -Handles HTTP requests and responses.
  -Should not contain business logic — only calls Application layer.

Dependency Rule
  -Dependencies always point inward:
  -API → Application → Domain
  -Infrastructure → Application → Domain
  -Domain depends on nothing.

Interview Explanation (30 sec answer)
  -Clean Architecture keeps business logic independent from frameworks so database, UI, or cloud can change without affecting core rules. It improves    testability, maintainability, and long-term scalability.

# Interviewer asks:
  -Why private setters?
# You answer:
  -To protect business invariants. Device state should change only through domain behavior methods like ReceiveHeartbeat(), not random external modification.
