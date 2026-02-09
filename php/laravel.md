# 📘 CURSOR PROMPTS – Laravel

**Rules • Prompts • Debug cho Laravel framework trong Cursor IDE**

> File này thuộc thư mục [php/](./README.md).

---

## Mục lục

- [1. Rules cho Laravel](#1-rules-cho-laravel)
- [2. Model & Database](#2-model--database)
- [3. Controller & API](#3-controller--api)
- [4. Authentication & Authorization](#4-authentication--authorization)
- [5. Jobs, Events & Queues](#5-jobs-events--queues)
- [6. Middleware & Service Providers](#6-middleware--service-providers)
- [7. Eloquent Optimization](#7-eloquent-optimization)
- [8. Debug Prompts](#8-debug-prompts)
- [9. Testing Prompts](#9-testing-prompts)
- [10. Best Practices](#10-best-practices)

---

## 1. Rules cho Laravel

### 1.1 File `.cursor/rules/laravel.mdc`

```
---
description: Rules for Laravel application files
globs: app/**/*.php, routes/**/*.php, database/**/*.php, tests/**/*.php
---

- Follow Laravel conventions (naming, directory structure, patterns)
- Use Eloquent ORM with eager loading (avoid N+1)
- Use Form Request classes for validation
- Use API Resources for response formatting
- Use Policies for authorization
- Use Events/Listeners for decoupled logic
- Use Jobs for heavy/async processing
- Use strict types: declare(strict_types=1)
- Type declarations on all methods
- Use Laravel 11 structure (bootstrap/app.php, no Kernel)
- Use Facades sparingly, prefer DI when testing matters
```

### 1.2 Full `.cursorrules` cho Laravel

```txt
You are an expert Laravel developer with deep knowledge of Laravel 11,
PHP 8.3, Eloquent, and the Laravel ecosystem.

# Laravel Architecture
- Follow MVC + Service pattern
- Controllers: thin, delegate to Services
- Services: business logic
- Repositories: data access (optional, Eloquent is often enough)
- Form Requests: validation
- API Resources: response transformation
- Policies: authorization
- Events/Listeners: decoupled side effects
- Jobs: async processing

# Eloquent
- Eager loading: with() / load() to prevent N+1
- Scopes for reusable query logic
- Casts for type conversion
- Accessors/Mutators with Attribute class
- Use chunking for large datasets

# Naming Conventions
- Models: singular PascalCase (User, OrderItem)
- Controllers: PascalCase + Controller suffix
- Migrations: snake_case with timestamp
- Routes: kebab-case or snake_case
- Blade: kebab-case (user-profile.blade.php)
```

---

## 2. Model & Database

### 2.1 Model + Migration

```text
@codebase

Create Laravel model for [entity]:

Requirements:
- Model with: $fillable, $casts, relationships
- Migration with proper column types, indexes, foreign keys
- Factory for testing
- Seeder with realistic data
- Relationships: [belongsTo, hasMany, belongsToMany, morphTo, etc.]
- Scopes for common queries
- Accessors/Mutators (Attribute class)
- PHP 8+ attribute syntax for casts

Generate: php artisan make:model [Name] -mfsc
```

### 2.2 Complex Relationships

```text
@codebase

Set up relationships for [mô tả entity graph]:

Requirements:
- Define all relationships on both sides
- Eager loading defaults ($with if always needed)
- Pivot tables for many-to-many (with pivot data if needed)
- Polymorphic relationships if applicable
- Has-through relationships
- Cascade delete/soft delete considerations
```

### 2.3 Migration best practices

```text
@codebase

Create migration for [schema change]:

Requirements:
- Schema::create or Schema::table
- Proper column types and modifiers
- Indexes for queried columns
- Foreign key constraints with onDelete behavior
- Reversible (down method that undoes up)
- No data loss
- Safe for production (no locking long tables)
```

### 2.4 Seeder + Factory

```text
@codebase

Create test data for [entity]:

Requirements:
- Factory with realistic fake data
- Different states: (->state('active'), ->state('admin'))
- Relationship creation in factory (->has(), ->for())
- Seeder that creates meaningful dataset
- DatabaseSeeder registration
```

---

## 3. Controller & API

### 3.1 Resource Controller + API Resource

```text
@codebase

Create full API resource for [entity]:

Files:
- Controller (php artisan make:controller --api)
- Form Requests (Store + Update)
- API Resource + Resource Collection
- Routes (api.php)
- Policy (if auth needed)

Endpoints:
- GET /api/[resources] – index (paginate, filter, sort, search)
- GET /api/[resources]/{id} – show
- POST /api/[resources] – store (validate, 201)
- PUT /api/[resources]/{id} – update (validate)
- DELETE /api/[resources]/{id} – destroy (204)

Requirements:
- Form Request validation (rules, messages, authorize)
- API Resource for response (don't expose model directly)
- Proper HTTP status codes
- Pagination with meta info
- Filter with query parameters
- Sort with ?sort=field&direction=asc
- Search with ?search=term
```

### 3.2 Form Request Validation

```text
@codebase

Create Form Request for [action on entity]:

Requirements:
- php artisan make:request [Name]Request
- authorize() method
- rules() with proper validation rules
- messages() for custom error messages
- prepareForValidation() for data transformation
- After validation hook if complex logic
- Conditional rules (sometimes, when)
- Array/nested validation if needed
```

### 3.3 API Resource Transformation

```text
@codebase

Create API Resource for [entity]:

Requirements:
- JsonResource for single item
- ResourceCollection for collections
- Conditional attributes (when, mergeWhen)
- Nested resources for relationships
- Pagination metadata
- Additional meta/links
```

### 3.4 Invokable Controller

```text
@codebase

Create invokable controller for [single action]:
- __invoke method
- Single responsibility
- Form Request if needs validation
- Route registration
```

---

## 4. Authentication & Authorization

### 4.1 API Authentication (Sanctum)

```text
@codebase

Implement API auth with Laravel Sanctum:

Requirements:
- User registration with validation
- Login → token generation
- Logout → token revocation
- Token abilities/scopes
- Protected routes (auth:sanctum)
- Rate limiting on auth endpoints
```

### 4.2 Policy Authorization

```text
@codebase

Create Policy for [model]:

Requirements:
- php artisan make:policy [Name]Policy --model=[Model]
- viewAny, view, create, update, delete, restore, forceDelete
- Authorization logic per method
- Register in AuthServiceProvider (or auto-discovery)
- Use in controller ($this->authorize, Gate::allows)
- Use in Blade (@can, @cannot)
```

### 4.3 Role-based Access

```text
@codebase

Implement role/permission system:

Requirements:
- Use [Spatie Permission / custom implementation]
- Role model + Permission model
- User has roles, roles have permissions
- Middleware for route protection
- Blade directives (@role, @hasPermission)
- Super admin bypass
- Seeder for default roles/permissions
```

---

## 5. Jobs, Events & Queues

### 5.1 Queued Job

```text
@codebase

Create queued job for [task: send email, process data, generate report]:

Requirements:
- implements ShouldQueue
- Constructor with serializable data
- handle() with business logic
- tries, backoff, maxExceptions configuration
- failed() method for error handling
- Unique jobs (ShouldBeUnique) if needed
- Rate limiting if needed
- Dispatch from controller/service
```

### 5.2 Event + Listener

```text
@codebase

Create event-driven flow for [trigger: user registered, order placed, payment received]:

Requirements:
- Event class with relevant data
- Listener class(es)
- ShouldQueue on listener for async processing
- EventServiceProvider registration (or auto-discovery)
- ShouldBroadcast if real-time (Pusher/Reverb)
- Test event dispatching
```

### 5.3 Notification

```text
@codebase

Create notification for [trigger]:

Requirements:
- via(): [mail, database, broadcast, slack]
- toMail(): Mailable or MailMessage
- toDatabase(): array for storage
- toBroadcast(): if real-time
- Queueable (ShouldQueue)
- On-demand notification if needed
```

### 5.4 Scheduled Task

```text
@codebase

Create scheduled task for [recurring job]:

Requirements:
- Artisan command or closure
- Schedule definition in routes/console.php (Laravel 11)
- Proper frequency (daily, hourly, everyMinute, cron)
- Overlap prevention (withoutOverlapping)
- Output logging
- Failure notification
- Health check (schedule:run monitoring)
```

---

## 6. Middleware & Service Providers

### 6.1 Middleware

```text
@codebase

Create middleware for [feature: API throttle, locale, CORS, JSON response, admin check]:

Requirements:
- handle() method
- Register in bootstrap/app.php (Laravel 11)
- Named middleware or global
- Terminate method (if post-response processing needed)
```

### 6.2 Service Provider

```text
@codebase

Create service provider for [feature: external API, config, bindings]:

Requirements:
- register(): bind interfaces to implementations
- boot(): event listeners, observers, macros
- Deferred loading if heavy initialization
- Register in config/app.php or bootstrap/providers.php
```

---

## 7. Eloquent Optimization

### 7.1 Fix N+1

```text
@codebase
@file [filepath]

Fix N+1 query issues:

Please:
- Add eager loading: with() or load()
- Use withCount() for counting related
- Use subquery selects for computed columns
- Add preventLazyLoading() in dev for detection
- Show query count before/after
```

### 7.2 Query Optimization

```text
@codebase

Optimize Eloquent queries for [feature]:

Please:
- Use select() to limit columns
- Use cursor() for memory-efficient iteration
- Use chunk() or chunkById() for batch processing
- Add missing database indexes
- Use raw expressions for complex aggregations
- Cache frequently accessed queries
- Show SQL generated with toSql()
```

---

## 8. Debug Prompts

### 8.1 500 Error

```text
@terminal
@file storage/logs/laravel.log

500 error: [paste from log]
Fix root cause, add error handling.
```

### 8.2 N+1 Query

```text
@codebase

Laravel Debugbar shows N+1:
[paste query count or describe]

Fix with eager loading.
```

### 8.3 Migration Error

```text
@terminal

php artisan migrate error:
[paste error]

Fix migration, check for conflicts.
```

### 8.4 Auth Issue

```text
@codebase

Auth issue: [describe: can't login, 401, 403, session lost]
Check: guard config, middleware, token, session driver.
```

### 8.5 Queue / Job Failure

```text
@terminal
@codebase

Job failed:
[paste from failed_jobs table or log]

Fix: check handle() logic, retry config, serialization issues.
```

---

## 9. Testing Prompts

### 9.1 Feature Tests

```text
@codebase

Write feature tests for [API endpoint / page]:

Requirements:
- RefreshDatabase trait
- Factories for test data
- Test each HTTP method + status code
- Test validation (422)
- Test auth (401/403)
- Test pagination, filter, sort
- assertJson / assertJsonStructure
- assertDatabaseHas / assertDatabaseMissing
```

### 9.2 Unit Tests

```text
@codebase

Write unit tests for [service / model]:
- Mock dependencies
- Test business logic
- Test edge cases
- Test exceptions
```

### 9.3 Pest Tests

```text
@codebase

Write Pest tests for [feature]:
- it('should ...') syntax
- expect() chains
- Datasets for parametric
- beforeEach for setup
```

### 9.4 Dusk Browser Tests

```text
@codebase

Write Dusk tests for [page/flow]:
- Login, navigate, fill form, assert
- Wait for async elements
- Screenshot on failure
```

---

## 10. Best Practices

### ✅ DO
- Eager loading (`with()`) để tránh N+1
- Form Request cho validation | API Resource cho response
- Policy cho authorization | Events cho side effects
- Jobs cho heavy processing | Queues cho async
- `declare(strict_types=1)` | Type declarations mọi nơi
- Factory + Seeder cho test data
- `config()` / `env()` cho settings (không hardcode)
- Cache config/routes/views trong production

### ❌ DON'T
- Không business logic trong Controllers (dùng Service)
- Không return Eloquent model trực tiếp từ API (dùng Resource)
- Không query trong Blade templates
- Không `env()` ngoài config files (cached sẽ return null)
- Không skip validation | Không trust client data
- Không commit `.env` | Không `dd()` trong production
- Không `DB::raw()` với user input
- Không circular event/listener triggers

---

> 📌 Quay lại [PHP Core](./README.md) | [Handbook chính](../cursor-ide-handbook.md)
