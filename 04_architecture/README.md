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




Domain Entity vs Database Table
Simple Explanation
A domain entity represents business behavior, not just stored data.
It contains rules and actions that protect system correctness.

Example
Device status should not be manually set.
It must be calculated from last heartbeat time.
So instead of storing IsOffline column, we calculate:
IsOffline = Now - LastHeartbeat > threshold

Interview Explanation
In good design, entities encapsulate behavior and protect invariants.
We don’t allow external layers to directly modify state because it can break business rules. Instead, we expose methods like ReceiveHeartbeat().

Why Private Setters?
Prevents invalid state changes from outside layers like controllers or repositories.

Common Mistake
CRUD model:
public bool IsOffline { get; set; }
This allows anyone to mark device online/offline incorrectly.





Use Case / Command Handler (CQRS Style)
Simple Explanation
Instead of controllers directly updating database, we create a use case class that represents a business action.
Controller triggers intention → handler executes business logic.

Why Not Put Logic in Controller?
Controllers are delivery mechanism (HTTP).
Business rules should be reusable from background jobs, queues, or other APIs.

Flow
Controller → Command → Handler → Domain → Repository

Interview Explanation
We separate request handling from business execution using command handlers. This keeps the application independent of transport layer and improves testability and reuse.

Example
RegisterDeviceHandler creates device entity and persists it through repository instead of controller directly calling DB.

Common Mistake
Fat Controllers:
Controller contains validation, DB calls, business logic, and response mapping.
