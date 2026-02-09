# 📘 CURSOR PROMPTS – C# Core

**Rules • Prompts • Debug • Best Practices cho C# trong Cursor IDE**

> Thư mục này là phần mở rộng của [cursor-ide-handbook.md](../cursor-ide-handbook.md) – Section 11.

---

## Cấu trúc thư mục

```
csharp/
├── README.md              ← Bạn đang ở đây (C# core)
└── aspnet.md              ← ASP.NET Core Web API, MVC, Blazor
```

---

## Mục lục

- [1. Rules template cho C#](#1-rules-template-cho-c)
- [2. Prompt Library – C# Core](#2-prompt-library--c-core)
- [3. Entity Framework Core](#3-entity-framework-core)
- [4. Debug Prompts](#4-debug-prompts)
- [5. Testing Prompts](#5-testing-prompts)
- [6. Architecture & Patterns](#6-architecture--patterns)
- [7. Best Practices & Anti-patterns](#7-best-practices--anti-patterns)

---

## 1. Rules template cho C#

### 1.1 File `.cursor/rules/csharp.mdc`

```
---
description: Rules for C# files
globs: *.cs
---

- Write modern C# (12.0+, .NET 8)
- Follow .NET naming conventions (PascalCase public, _camelCase private)
- Enable nullable reference types, handle null properly
- Use async/await for I/O, never .Result or .Wait()
- Pass CancellationToken through async chains
- Use constructor injection for DI
- Use specific exception types, not bare catch {}
- Use LINQ methods, avoid N+1 queries
- Implement IDisposable/IAsyncDisposable for unmanaged resources
- Add XML doc comments for public APIs
- Use record types for DTOs and value objects
- Use pattern matching where it improves readability
```

### 1.2 Full `.cursorrules`

```txt
You are an expert C# / .NET developer with deep knowledge of .NET 8,
async programming, Entity Framework Core, and modern C# patterns.

# Naming: PascalCase public, _camelCase private, IPrefix interfaces
# Nullable: #nullable enable, handle null properly
# Async: async/await for I/O, CancellationToken everywhere, never .Result/.Wait()
# DI: constructor injection, interfaces for services
# Errors: specific exceptions, structured logging, no bare catch {}
# LINQ: methods over query syntax, AsNoTracking for reads
```

---

## 2. Prompt Library – C# Core

### 2.1 Tạo Service class

```text
@codebase

Create a C# service:
- Interface (IXxxService) + Implementation (XxxService)
- Constructor injection, async methods with CancellationToken
- Error handling with logging
- XML doc comments
- Register in DI container
```

### 2.2 Tạo DTO / Record

```text
@codebase

Create DTOs for [entity]:
- record types (immutable)
- Validation attributes (Required, MaxLength, Range)
- Mapping methods or AutoMapper profile
- Nullable reference types enabled
```

### 2.3 Repository Pattern

```text
@codebase

Implement repository for [entity]:
- Interface + EF Core implementation
- Async methods with CancellationToken
- AsNoTracking for reads
- Proper Include/ThenInclude
```

### 2.4 CQRS with MediatR

```text
@codebase

Implement CQRS for [feature]:
- Commands + Queries + Handlers
- FluentValidation for input
- Pipeline behaviors (logging, validation)
- Return Result<T> pattern
```

### 2.5 Background Service

```text
@codebase

Create BackgroundService for [task]:
- Proper cancellation support
- Error handling with retry
- Scoped service injection (IServiceScopeFactory)
- Health check + logging
```

---

## 3. Entity Framework Core

### 3.1 Entity + Configuration

```text
@codebase

Create EF Core entity for [entity]:
- Entity class + IEntityTypeConfiguration (Fluent API)
- Relationships, indexes
- Audit fields (CreatedAt, UpdatedAt)
- Soft delete support if project uses it
```

### 3.2 Complex Query

```text
@codebase
@file [DbContext]

EF Core query for [mô tả]:
- LINQ methods, no N+1
- AsNoTracking for reads
- Projection (Select) to avoid loading unnecessary columns
- Pagination with Skip/Take
```

---

## 4. Debug Prompts

### 4.1 NullReferenceException

```text
@terminal
@file [filepath]

NullReferenceException: [paste stack trace]
Fix: add null checks, fix root cause, suggest #nullable enable.
```

### 4.2 DI Error

```text
@terminal
@codebase

DI error: [paste error]
Fix: missing registration, lifetime compatibility, circular dependency.
```

### 4.3 Async Deadlock

```text
@file [filepath]

Suspected deadlock: [paste or describe]
Fix: replace .Result/.Wait() with async/await, add CancellationToken.
```

---

## 5. Testing Prompts

### 5.1 Unit tests (xUnit)

```text
@codebase

Write xUnit tests for @file [filepath]:
- Arrange-Act-Assert
- Mock with Moq/NSubstitute
- Test happy path + edge cases + errors
- Async tests with proper await
```

### 5.2 Integration tests

```text
@codebase

Write integration tests:
- WebApplicationFactory<Program>
- In-memory database or Testcontainers
- Test full request pipeline + auth + validation
```

---

## 6. Architecture & Patterns

### 6.1 Clean Architecture

```text
@codebase

Set up Clean Architecture:
1. Domain (entities, interfaces, domain events)
2. Application (use cases, DTOs, validators)
3. Infrastructure (EF Core, external services)
4. API (controllers, middleware)

Dependency rule: inner layers don't reference outer.
```

### 6.2 DDD Patterns

```text
@codebase

Implement DDD for [aggregate]:
- Aggregate root, value objects, domain events
- Repository interface in Domain
- Factory methods, domain services
```

---

## 7. Best Practices & Anti-patterns

### ✅ DO
- `async/await` cho I/O | `CancellationToken` xuyên suốt
- `record` cho DTOs | `#nullable enable`
- Constructor injection | `ILogger<T>` structured logging
- Pattern matching | `AsNoTracking()` cho EF reads
- `using declaration` | XML doc cho public API

### ❌ DON'T
- Không `.Result` / `.Wait()` | Không bare `catch {}`
- Không static mutable state | Không service locator
- Không string concatenation cho SQL
- Không `DateTime.Now` – dùng `DateTime.UtcNow` / `TimeProvider`
- Không `HttpClient` trực tiếp – dùng `IHttpClientFactory`

---

## Xem thêm

| Chủ đề | File |
|--------|------|
| **ASP.NET Core** | [aspnet.md](./aspnet.md) |

> 📌 Quay lại [Handbook chính](../cursor-ide-handbook.md)
