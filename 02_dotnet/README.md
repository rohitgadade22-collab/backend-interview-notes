EF Core Change Tracking
Simple
EF tracks loaded entities in memory. When properties change, EF automatically updates DB on SaveChanges().

Important
We modify entity instead of creating new one so EF can detect changes.

Interview Answer
EF Core uses change tracking to monitor entity state. When a tracked entity is mutated, EF generates an UPDATE statement automatically during SaveChanges.
