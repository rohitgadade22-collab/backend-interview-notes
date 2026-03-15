# Digital Banking Backend – Interview Questions & Concepts

This project demonstrates a backend banking system implemented using **ASP.NET Core, Entity Framework Core, Clean Architecture, CQRS, and Repository Pattern**.
Below are some important technical concepts and interview questions that can be explained using this project.

---

# 1. What is Dependency Injection and how did you use it?

**Answer**

Dependency Injection (DI) is a design pattern used to achieve **loose coupling** between classes by injecting dependencies instead of creating them inside the class.

In this project, repositories are injected into handlers using constructor injection.

Example:

```csharp
private readonly ITransactionHistoryRepository _repository;

public GetTransactionHistoryQueryHandler(ITransactionHistoryRepository repository)
{
    _repository = repository;
}
```

ASP.NET Core automatically provides the repository instance through its built-in **DI container**.

---

# 2. What is the Repository Pattern?

**Answer**

The Repository Pattern abstracts database operations from the business logic layer.

Instead of directly using Entity Framework in the application layer, database operations are handled through repository interfaces.

Example:

```csharp
public interface ITransactionHistoryRepository
{
    Task<IEnumerable<TransactionHistory>> GetByAccountIdAsync(Guid accountId, CancellationToken cancellationToken);
}
```

Benefits:

* Separation of concerns
* Easier unit testing
* Cleaner architecture

---

# 3. What is CQRS and how is it used in this project?

**Answer**

CQRS (Command Query Responsibility Segregation) separates **read operations** from **write operations**.

Commands → change system state
Queries → retrieve data

Example:

Commands:

```
TransferMoneyCommand
TransferMoneyCommandHandler
```

Queries:

```
GetTransactionHistoryQuery
GetTransactionHistoryQueryHandler
```

This improves maintainability and scalability.

---

# 4. What are Navigation Properties in Entity Framework?

**Answer**

Navigation properties allow accessing related entities through foreign key relationships.

Example from the project:

```csharp
public Account? FromAccount { get; private set; }
public Account? ToAccount { get; private set; }
```

EF Core loads these relationships using:

```csharp
.Include(t => t.FromAccount)
.Include(t => t.ToAccount)
```

This allows access to related properties like `AccountNumber`.

---

# 5. Why are DTOs used in APIs?

**Answer**

DTOs (Data Transfer Objects) are used to control what data is returned to the client.

Instead of exposing database entities directly, DTOs allow shaping the response.

Example:

```csharp
public class TransactionHistoryDto
{
    public string ReferenceNumber { get; set; }
    public decimal Amount { get; set; }
    public string Direction { get; set; }
    public string CounterpartyAccountNumber { get; set; }
}
```

Benefits:

* Hide internal database structure
* Improve API security
* Provide clean response models

---

# 6. What is the difference between IEnumerable and IQueryable?

**Answer**

| IEnumerable                    | IQueryable                   |
| ------------------------------ | ---------------------------- |
| Works on in-memory collections | Works on database queries    |
| LINQ executed in memory        | LINQ translated into SQL     |
| Data already loaded            | Query built before execution |

Example:

```csharp
_context.TransactionHistories.Where(...)
```

This uses **IQueryable** and EF Core converts it into SQL.

---

# 7. What are Navigation Includes in EF Core?

**Answer**

Include() loads related entities from the database.

Example:

```csharp
_context.TransactionHistories
    .Include(t => t.FromAccount)
    .Include(t => t.ToAccount)
```

This ensures related accounts are loaded with the transaction.

Equivalent SQL:

```
JOIN Accounts ON TransactionHistories.FromAccountId = Accounts.Id
```

---

# 8. What is the Null Conditional Operator?

**Answer**

The `?.` operator allows safe access to properties when the object might be null.

Example:

```csharp
t.ToAccount?.AccountNumber
```

Meaning:

* If `ToAccount` is null → return null
* Otherwise → return `AccountNumber`

This prevents **NullReferenceException**.

---

# 9. What is the Ternary Operator?

**Answer**

The ternary operator is a shorthand conditional expression.

Syntax:

```
condition ? value_if_true : value_if_false
```

Example from the project:

```csharp
Direction = t.FromAccountId == query.AccountId ? "Debit" : "Credit";
```

If the account matches the sender → Debit
Otherwise → Credit

---

# 10. How do you ensure transaction consistency in a banking system?

**Answer**

Financial operations must be **atomic**. Debit, credit, and transaction history creation must either succeed together or fail together.

This is handled using database transactions.

Example:

```csharp
using var transaction = await _context.Database.BeginTransactionAsync();
```

If any step fails:

```
Rollback transaction
```

Otherwise:

```
Commit transaction
```

This ensures **data consistency**.

---

# Technologies Used

* ASP.NET Core
* Entity Framework Core
* Clean Architecture
* CQRS
* LINQ
* Dependency Injection
* SQL Server
* Repository Pattern

---

# Summary

This project demonstrates several important backend development concepts including:

* Dependency Injection
* CQRS
* Repository Pattern
* EF Core Relationships
* DTO Mapping
* Database Transactions
* LINQ Queries
* Navigation Properties
* Async Programming

These concepts are commonly used in **real-world banking and financial backend systems**.
