Code First Migration (EF Core)
Simple Explanation
Database schema is generated from domain models instead of manually writing SQL tables.

Why Useful
Keeps database synchronized with application models and avoids manual schema mistakes.

Flow
Entity → DbContext → Migration → Database

Interview Explanation
We used EF Core Code-First approach so schema evolves with domain model. Migrations provide version control for database structure and safe deployments across environments.

Advantage
DB structure tracked in source control
Easy deployment to staging/production
No manual SQL scripts required

Common Question
Q: What if production DB already has data?
A: We apply incremental migrations — EF only alters required columns without data loss.
