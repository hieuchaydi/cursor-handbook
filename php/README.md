# 📘 CURSOR PROMPTS – PHP Thuần (Core PHP)

**Rules • Prompts • Debug • Best Practices cho PHP thuần trong Cursor IDE**

> Thư mục này là phần mở rộng của [cursor-ide-handbook.md](../cursor-ide-handbook.md) – Section 11.

---

## Cấu trúc thư mục

```
php/
├── README.md              ← Bạn đang ở đây (PHP thuần)
└── laravel.md             ← Laravel framework
```

---

## Mục lục

- [1. Rules template cho PHP thuần](#1-rules-template-cho-php-thuần)
- [2. Prompt Library – PHP Core](#2-prompt-library--php-core)
- [3. OOP & Design Patterns](#3-oop--design-patterns)
- [4. Database (PDO)](#4-database-pdo)
- [5. Security Prompts](#5-security-prompts)
- [6. Debug Prompts](#6-debug-prompts)
- [7. Testing Prompts](#7-testing-prompts)
- [8. Best Practices & Anti-patterns](#8-best-practices--anti-patterns)

---

## 1. Rules template cho PHP thuần

### 1.1 File `.cursor/rules/php.mdc`

```
---
description: Rules for PHP files
globs: *.php
---

- Write modern PHP 8.2+ with declare(strict_types=1)
- Follow PSR-12 coding standard
- Type declarations for ALL parameters, return types, properties
- Use enum for fixed sets of values
- Use readonly properties for immutable data
- Use prepared statements for SQL (never string concatenation)
- Validate and sanitize ALL user input
- Throw specific exception types
- Use Composer autoloading (PSR-4)
- Add PHPDoc for complex return types
- Use dependency injection, not global state
```

### 1.2 Full `.cursorrules` cho PHP project

```txt
You are an expert PHP developer with deep knowledge of PHP 8.3+,
OOP, design patterns, and modern PHP best practices.

# General
- Modern PHP 8.2+, declare(strict_types=1) everywhere
- PSR-12 coding standard
- Type declarations for ALL parameters, return types, properties
- Composer autoloading (PSR-4)
- No global state, dependency injection

# Type System
- Union types (string|int), intersection types, nullable types (?string)
- readonly properties, enum, match expressions
- Named arguments where it improves readability

# Error Handling
- Specific exception types, never suppress with @
- Custom exceptions for domain logic

# Security
- Prepared statements for SQL, validate all input
- password_hash() / password_verify() for passwords
- htmlspecialchars() for output escaping
- CSRF protection for forms

# Output Format
- "Files changed/created:" at top
- Code blocks with 'php' identifier
```

---

## 2. Prompt Library – PHP Core

### 2.1 Tạo class mới

```text
@codebase

Create a PHP class for [mô tả]:

Requirements:
- declare(strict_types=1)
- Type declarations for all properties, parameters, return types
- Constructor with proper DI
- readonly properties where appropriate
- PHPDoc for complex methods
- PSR-4 namespace
```

### 2.2 Tạo Service / Repository

```text
@codebase

Create a service class for [business logic]:

Requirements:
- Interface + implementation
- Constructor injection
- Type-safe methods
- Exception handling with custom exceptions
- Logging critical operations
- Unit testable (no static dependencies)
```

### 2.3 Tạo DTO / Value Object

```text
@codebase

Create DTOs for [feature]:

Requirements:
- readonly class (PHP 8.2)
- Constructor promotion
- Static factory: fromArray(), fromRequest()
- toArray() for serialization
- Validation logic
```

### 2.4 Refactor legacy PHP

```text
@file [filepath]

Modernize this PHP code:
- Add declare(strict_types=1)
- Add type declarations everywhere
- Replace array() with []
- Use match() instead of switch
- Use enum, readonly, named arguments
- Use str_contains/str_starts_with instead of strpos
- Use null-safe operator (?->)

Keep same behavior.
```

### 2.5 Router / MVC từ scratch

```text
@codebase

Create a simple MVC router:

Requirements:
- Route registration (GET, POST, PUT, DELETE)
- URL parameter extraction (/users/{id})
- Controller dispatching
- Middleware support
- 404/405 error handling
- PSR-7 compatible (optional)
```

### 2.6 Template engine đơn giản

```text
@codebase

Create a simple template engine:

Requirements:
- Variable substitution ({{ name }})
- Layout/extends system
- Include partials
- Auto-escaping (XSS prevention)
- Caching compiled templates
- Sections/blocks
```

---

## 3. OOP & Design Patterns

### 3.1 Design patterns

```text
@codebase

Implement [Singleton / Factory / Strategy / Observer / Repository / Decorator / Builder] in PHP:

Requirements:
- Modern PHP (interfaces, enums, readonly, match)
- Type-safe with generics-like patterns
- Example usage
- Unit test
```

### 3.2 Interface + Dependency Injection

```text
@codebase

Create interface-based architecture for [feature]:

Requirements:
- Interface definition
- Multiple implementations
- DI container registration (if using one)
- Type-safe injection
- Example wiring code
```

---

## 4. Database (PDO)

### 4.1 Database wrapper

```text
@codebase

Create a PDO database wrapper:

Requirements:
- Connection with DSN from config/env
- Prepared statement helper (query, fetch, fetchAll)
- Transaction support (begin, commit, rollback)
- Named parameters (:name) support
- Error handling (PDOException)
- Connection pooling consideration
- Type-safe result methods
```

### 4.2 Repository pattern (PDO)

```text
@codebase

Create repository for [entity] using PDO:

Requirements:
- CRUD operations with prepared statements
- Pagination (LIMIT/OFFSET)
- Filtering and sorting
- Transaction support
- Type-safe return (Entity objects, not arrays)
- No SQL injection possible
```

### 4.3 Migration system (simple)

```text
@codebase

Create a simple database migration system:

Requirements:
- Migration files with up() and down() methods
- Migration tracking table
- Run pending migrations
- Rollback last batch
- CLI commands: migrate, rollback, status
```

---

## 5. Security Prompts

### 5.1 SQL Injection Review

```text
@codebase
@file [filepath]

Review for SQL injection:
- Find raw SQL with user input
- Fix with prepared statements (PDO::prepare)
- Check dynamic table/column names
```

### 5.2 XSS Review

```text
@codebase

Review for XSS:
- Check unescaped output in HTML
- Fix with htmlspecialchars()
- Check JavaScript contexts
- Add Content-Security-Policy headers
```

### 5.3 Authentication Review

```text
@codebase

Review auth implementation:
- Password hashing (bcrypt via password_hash)
- Session management (regenerate ID on login)
- CSRF protection
- Brute force protection (rate limiting)
- Remember me security
```

### 5.4 File Upload Security

```text
@codebase

Review file upload:
- MIME type + extension validation
- File size limits
- Filename sanitization
- Storage outside web root
- No PHP execution in upload dir
```

---

## 6. Debug Prompts

### 6.1 Fatal Error / Exception

```text
@terminal
@file [filepath]

PHP error: [paste error]
Fix the error and add proper exception handling.
```

### 6.2 Session / Cookie Issue

```text
@codebase

Session issue: [describe]
Check: session_start placement, cookie config, session handler.
```

### 6.3 Composer Issue

```text
@terminal

Composer error: [paste error]
Fix: dependency conflict, version constraints, autoload.
```

---

## 7. Testing Prompts

### 7.1 PHPUnit tests

```text
@codebase

Write PHPUnit tests for @file [filepath]:
- Test each public method
- Mock dependencies with createMock()
- Data providers for multiple cases
- Test happy path + edge cases + exceptions
- @covers annotation
```

### 7.2 Pest PHP tests

```text
@codebase

Write Pest tests for [feature]:
- describe/it/test syntax
- Expectations: expect()->toBe(), etc.
- Datasets for parameterized tests
```

---

## 8. Best Practices & Anti-patterns

### ✅ DO
- `declare(strict_types=1)` mọi file
- Type declarations mọi nơi
- `readonly` cho immutable data | `enum` cho fixed sets
- Prepared statements cho SQL | `htmlspecialchars()` cho output
- `password_hash()` / `password_verify()`
- Composer autoloading (PSR-4)
- Dependency injection

### ❌ DON'T
- Không concatenate SQL với user input
- Không `@` suppress errors | Không `eval()` / `extract()`
- Không `md5()` / `sha1()` cho passwords
- Không `$_GET`/`$_POST` trực tiếp trong business logic
- Không `echo` trong service classes | Không `die()` / `exit()`
- Không `global` keyword | Không `mixed` type nếu tránh được

---

## Xem thêm

| Chủ đề | File |
|--------|------|
| **Laravel** | [laravel.md](./laravel.md) |

> 📌 Quay lại [Handbook chính](../cursor-ide-handbook.md)
