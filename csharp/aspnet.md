# 📘 CURSOR PROMPTS – ASP.NET Core

**Rules • Prompts • Debug cho ASP.NET Core Web API / MVC / Blazor trong Cursor IDE**

> File này thuộc thư mục [csharp/](./README.md).

---

## Mục lục

- [1. Rules cho ASP.NET Core](#1-rules-cho-aspnet-core)
- [2. Controller & API Prompts](#2-controller--api-prompts)
- [3. Minimal API](#3-minimal-api)
- [4. Authentication & Authorization](#4-authentication--authorization)
- [5. Middleware & Pipeline](#5-middleware--pipeline)
- [6. Real-time (SignalR)](#6-real-time-signalr)
- [7. Blazor Prompts](#7-blazor-prompts)
- [8. Debug Prompts](#8-debug-prompts)
- [9. Testing Prompts](#9-testing-prompts)
- [10. Best Practices](#10-best-practices)

---

## 1. Rules cho ASP.NET Core

### 1.1 File `.cursor/rules/aspnet.mdc`

```
---
description: Rules for ASP.NET Core files
globs: Controllers/**/*.cs, Endpoints/**/*.cs, Program.cs, Startup.cs, Middleware/**/*.cs
---

- Use [ApiController] attribute on controllers
- Async actions with CancellationToken
- Model validation with DataAnnotations or FluentValidation
- Return proper HTTP status codes (200, 201, 204, 400, 404, 500)
- Use ProblemDetails for error responses
- Use DTOs, never expose entities directly
- Use dependency injection for all services
- Swagger documentation (ProducesResponseType attributes)
- Authentication with [Authorize] attribute
- Use middleware for cross-cutting concerns
- Minimal API for simple endpoints, Controllers for complex ones
```

---

## 2. Controller & API Prompts

### 2.1 CRUD Controller

```text
@codebase

Create ASP.NET Core controller for [resource]:

Endpoints:
- GET /api/v1/[resources] – List (pagination, filter, sort)
- GET /api/v1/[resources]/{id} – Detail
- POST /api/v1/[resources] – Create (201 + Location header)
- PUT /api/v1/[resources]/{id} – Update
- DELETE /api/v1/[resources]/{id} – Delete (204)

Requirements:
- [ApiController] with route prefix
- Async with CancellationToken
- DTOs for request/response (no entity exposure)
- FluentValidation or DataAnnotations
- ProblemDetails for errors
- Swagger: [ProducesResponseType], [ApiTags]
- [Authorize] if needed
```

### 2.2 File Upload Controller

```text
@codebase

Create file upload endpoint:

Requirements:
- IFormFile parameter
- File size and type validation
- Storage: [local / Azure Blob / S3]
- Return file metadata
- Multipart upload support
- Progress tracking (if possible)
- Antivirus scanning consideration
```

### 2.3 Versioned API

```text
@codebase

Set up API versioning:

Requirements:
- Asp.Versioning.Http
- URL path versioning (/v1/, /v2/)
- Controller versioning: [ApiVersion("1.0")]
- Deprecation headers
- Swagger docs per version
```

---

## 3. Minimal API

### 3.1 Minimal API endpoints

```text
@codebase

Create Minimal API endpoints for [feature]:

Requirements:
- app.MapGroup() for route grouping
- TypedResults for response types
- Endpoint filters for validation
- OpenAPI docs (WithName, WithTags, Produces)
- CancellationToken support
- Authorization with RequireAuthorization()

Register in Program.cs.
```

### 3.2 Minimal API with Carter

```text
@codebase

Create Carter module for [feature]:

Requirements:
- ICarterModule implementation
- Route definitions in AddRoutes
- Request/response DTOs
- Validation with FluentValidation
- Dependency injection
```

---

## 4. Authentication & Authorization

### 4.1 JWT Authentication

```text
@codebase

Implement JWT authentication:

Requirements:
- JwtBearer configuration in Program.cs
- Login endpoint → access + refresh tokens
- Token generation service (claims, expiry)
- JwtBearerEvents for custom handling
- Refresh token endpoint + storage
- Password hashing (Identity or bcrypt)
- [Authorize] attribute on protected endpoints
```

### 4.2 Identity + Role-based Auth

```text
@codebase

Set up ASP.NET Identity with roles:

Requirements:
- AddIdentity configuration
- User registration + login + email confirmation
- Role management (Admin, User, etc.)
- [Authorize(Roles = "Admin")] usage
- Policy-based authorization for complex scenarios
- Claims-based authorization
```

### 4.3 OAuth2 / External Login

```text
@codebase

Add external login with [Google / Microsoft / GitHub]:

Requirements:
- AddAuthentication().AddGoogle/Microsoft/GitHub()
- Challenge + callback endpoints
- User linking (external login ↔ local account)
- JWT token generation after OAuth
```

---

## 5. Middleware & Pipeline

### 5.1 Custom Middleware

```text
@codebase

Create middleware for [exception handling / logging / correlation ID / rate limiting / caching]:

Requirements:
- IMiddleware or convention-based
- Async implementation
- Proper request/response handling
- Register in pipeline (app.UseXxx)
- Configuration options
- Logging
```

### 5.2 Global Exception Handler

```text
@codebase

Create global exception handling:

Requirements:
- ExceptionHandlerMiddleware or IExceptionHandler (.NET 8)
- Map exceptions → ProblemDetails responses
- Log errors with context
- Different responses for dev/prod
- Custom exception types → specific HTTP status codes
- Don't expose internal details in production
```

### 5.3 Request/Response Logging

```text
@codebase

Create request/response logging middleware:

Requirements:
- Log request: method, path, headers, body (sanitized)
- Log response: status code, duration
- Correlation ID tracking
- Exclude sensitive data (auth headers, passwords)
- Structured logging (Serilog / NLog)
- Configurable log level and path filtering
```

---

## 6. Real-time (SignalR)

### 6.1 SignalR Hub

```text
@codebase

Create SignalR hub for [chat / notifications / live dashboard]:

Requirements:
- Hub class with typed interface (IXxxHub)
- Group management (join, leave, broadcast)
- Authentication integration
- Connection lifecycle (OnConnectedAsync, OnDisconnectedAsync)
- Client method definitions
- Server-to-client push
- Error handling
- Scale-out with Redis backplane (if needed)
```

---

## 7. Blazor Prompts

### 7.1 Blazor Component

```text
@codebase

Create Blazor component for [mô tả]:

Requirements:
- [Parameter] properties
- EventCallback for parent communication
- Lifecycle methods (OnInitializedAsync, OnParametersSetAsync)
- Dependency injection (@inject)
- Error handling (ErrorBoundary)
- Loading states
- Render mode: [InteractiveServer / InteractiveWebAssembly / InteractiveAuto]
```

### 7.2 Blazor Form

```text
@codebase

Create Blazor EditForm for [mô tả]:

Requirements:
- EditForm with Model or EditContext
- DataAnnotationsValidator
- ValidationSummary / ValidationMessage per field
- Submit handling (OnValidSubmit)
- Loading state during submit
- Custom validation
```

---

## 8. Debug Prompts

### 8.1 500 Internal Server Error

```text
@terminal
@codebase

500 error: [paste stack trace or error details]
Check: exception type, middleware order, DI registration.
Fix and add proper error handling.
```

### 8.2 CORS Issue

```text
@codebase

CORS error: [paste browser error]
Fix: AddCors policy, UseCors placement in pipeline.
```

### 8.3 Auth / 401 / 403

```text
@codebase

Auth issue: [paste error]
Check: JWT config, token expiry, role/policy, [Authorize] placement.
```

### 8.4 Middleware Pipeline Order

```text
@codebase

Middleware issue: [paste error or describe]
Check: UseRouting → UseAuthentication → UseAuthorization → MapControllers order.
Fix pipeline ordering.
```

### 8.5 Performance Issue

```text
@codebase

API performance issue: [describe slow endpoint]
Check: N+1 queries, missing indexes, missing caching, blocking calls.
Profile with MiniProfiler or dotnet-trace.
```

---

## 9. Testing Prompts

### 9.1 Controller / API tests

```text
@codebase

Write integration tests for [endpoint]:
- WebApplicationFactory<Program>
- HttpClient for requests
- In-memory database
- Test status codes, response body, validation, auth
```

### 9.2 Middleware tests

```text
@codebase

Write tests for middleware [name]:
- Mock HttpContext, RequestDelegate
- Test request/response manipulation
- Test error handling
```

---

## 10. Best Practices

### ✅ DO
- `[ApiController]` + route attributes
- Async actions + `CancellationToken`
- DTOs for all request/response (never expose entities)
- `ProblemDetails` cho errors
- Global exception handler
- `[ProducesResponseType]` cho Swagger
- Health checks (`/healthz`)
- Structured logging (Serilog)
- HTTPS everywhere

### ❌ DON'T
- Không business logic trong Controllers
- Không expose Entity trực tiếp (dùng DTO)
- Không return `IQueryable` từ API
- Không hardcode connection strings
- Không skip model validation
- Không dùng `@Res()` / `HttpContext.Response.WriteAsync` trực tiếp (mất middleware features)

---

> 📌 Quay lại [C# Core](./README.md) | [Handbook chính](../cursor-ide-handbook.md)
