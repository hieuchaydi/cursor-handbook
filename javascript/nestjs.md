# 📘 CURSOR PROMPTS – NestJS

**Rules • Prompts • Debug • Best Practices cho NestJS trong Cursor IDE**

> File này thuộc thư mục [javascript/](./README.md) – hệ sinh thái JS/TS.

---

## Mục lục

- [1. Rules template cho NestJS](#1-rules-template-cho-nestjs)
- [2. Module & Service Prompts](#2-module--service-prompts)
- [3. Controller & API Prompts](#3-controller--api-prompts)
- [4. Database & ORM Prompts](#4-database--orm-prompts)
- [5. Authentication & Authorization](#5-authentication--authorization)
- [6. Advanced Patterns](#6-advanced-patterns)
- [7. Debug Prompts](#7-debug-prompts)
- [8. Testing Prompts](#8-testing-prompts)
- [9. Best Practices & Anti-patterns](#9-best-practices--anti-patterns)

---

## 1. Rules template cho NestJS

### 1.1 File `.cursor/rules/nestjs.mdc`

```
---
description: Rules for NestJS backend files
globs: src/**/*.ts, test/**/*.ts
---

- Follow NestJS modular architecture (Module → Controller → Service → Repository)
- Use decorators properly (@Controller, @Injectable, @Module, @Get, @Post, etc.)
- Constructor injection for all dependencies
- Use DTOs with class-validator decorators for input validation
- Use Pipes for transformation and validation
- Use Guards for authentication/authorization
- Use Interceptors for cross-cutting concerns (logging, caching, transform)
- Use custom Exception Filters for error handling
- Async methods returning Promises
- TypeScript strict mode, no 'any'
- Follow existing project patterns for naming and file organization
- One class per file, filename matches class name (kebab-case)
```

### 1.2 Full `.cursorrules` cho NestJS project

```txt
You are an expert NestJS developer with deep knowledge of NestJS 10+,
TypeScript, Node.js, and enterprise backend architecture patterns.

# NestJS Architecture
- Modular architecture: Module → Controller → Service → Repository.
- One module per domain feature.
- Controllers handle HTTP, Services handle business logic.
- Repositories handle data access (if using repository pattern).
- DTOs for input validation, Entities for database models.

# Dependency Injection
- Constructor injection for all dependencies.
- Use interfaces + custom providers when needed.
- Register providers in module @Module({ providers: [...] }).
- Use appropriate scopes: DEFAULT (singleton), REQUEST, TRANSIENT.

# Input Validation
- DTOs with class-validator decorators.
- Global ValidationPipe with whitelist and transform.
- Custom decorators for complex validation.
- Separate Create and Update DTOs.

# Error Handling
- Use NestJS built-in exceptions (NotFoundException, BadRequestException, etc.).
- Custom exception filters for specific error types.
- Never expose internal errors to clients.
- Return consistent error response format.

# Security
- Guards for authentication and authorization.
- Helmet for HTTP headers.
- CORS configuration.
- Rate limiting (ThrottlerModule).
- Input sanitization.
- No secrets in code – use ConfigModule + .env.

# TypeScript
- Strict mode, no 'any'.
- Interfaces for complex types.
- Enums for fixed sets.
- Generic types for reusable patterns.

# Output Format
- Always start with: "Files changed/created:"
- List each file path clearly.
- Explain module registration if new providers/modules added.
```

---

## 2. Module & Service Prompts

### 2.1 Tạo module đầy đủ

```text
@codebase

Create a NestJS module for [feature/domain]:

Structure:
- [feature].module.ts
- [feature].controller.ts
- [feature].service.ts
- dto/create-[feature].dto.ts
- dto/update-[feature].dto.ts
- dto/[feature]-response.dto.ts
- entities/[feature].entity.ts
- [feature].repository.ts (if using repository pattern)

Requirements:
- Module with proper imports/exports/providers
- Register in AppModule
- TypeScript strict types
- class-validator on DTOs
- Proper error handling

Follow existing project structure.
```

### 2.2 Tạo Service

```text
@codebase

Create a NestJS service for [mô tả business logic].

Requirements:
- @Injectable() decorator
- Constructor injection for dependencies
- Async methods with proper return types
- Error handling with NestJS exceptions
- Logging with Logger
- Input validation (or delegate to pipe/DTO)
- Transaction support for multi-step operations
- Unit testable (mockable dependencies)
```

### 2.3 Tạo Provider tùy chỉnh

```text
@codebase

Create a custom provider for [mô tả: external service client, config, factory].

Requirements:
- useFactory / useClass / useValue as appropriate
- Async factory if initialization is async
- Injection token (string or Symbol)
- Interface for type safety
- Register in module
- Export if shared across modules
- Cleanup on application shutdown (onModuleDestroy)
```

### 2.4 Dynamic Module

```text
@codebase

Create a dynamic module for [mô tả: configurable service, multi-tenant, plugin system].

Requirements:
- Static register() / registerAsync() methods
- forRoot() for global registration
- forFeature() for feature-specific configuration
- Options interface with defaults
- Async options support (useFactory)
- Global module option (if applicable)
```

---

## 3. Controller & API Prompts

### 3.1 CRUD Controller

```text
@codebase

Create a NestJS CRUD controller for [resource]:

Endpoints:
- GET /[resource] – List with pagination, filtering, sorting
- GET /[resource]/:id – Get by ID
- POST /[resource] – Create
- PATCH /[resource]/:id – Partial update
- DELETE /[resource]/:id – Delete

Requirements:
- @Controller() with proper route prefix
- TypeScript-typed parameters (@Param, @Query, @Body)
- DTOs with class-validator for Create and Update
- Response DTOs (don't expose entity directly)
- Swagger decorators (@ApiTags, @ApiOperation, @ApiResponse)
- Guards for authentication
- Proper HTTP status codes (@HttpCode)
- Interceptors for response transformation
```

### 3.2 File Upload endpoint

```text
@codebase

Create a file upload endpoint for [mô tả].

Requirements:
- @UseInterceptors(FileInterceptor('file'))
- Multer configuration (file size, type validation)
- Storage configuration (local / S3 / cloud)
- File validation pipe (custom)
- Return file metadata in response
- Error handling for invalid files
- Multiple file upload option
- Swagger documentation
```

### 3.3 SSE / Streaming endpoint

```text
@codebase

Create a Server-Sent Events endpoint for [mô tả: notifications, live updates].

Requirements:
- @Sse() decorator or Observable response
- Event formatting (id, type, data)
- Connection management
- Heartbeat to keep connection alive
- Authentication
- Graceful cleanup on disconnect
- Client-side EventSource example
```

### 3.4 Versioned API

```text
@codebase

Set up API versioning for [feature]:

Requirements:
- URI versioning (/v1/, /v2/) or Header versioning
- Controller versioning with @Controller({ version: '1' })
- Shared and version-specific logic
- Deprecation headers for old versions
- Documentation per version
```

---

## 4. Database & ORM Prompts

### 4.1 TypeORM Entity + Repository

```text
@codebase

Create TypeORM entity and repository for [entity]:

Requirements:
- Entity with decorators (@Entity, @Column, @PrimaryGeneratedColumn)
- Relationships (@ManyToOne, @OneToMany, @ManyToMany, @OneToOne)
- Indexes (@Index)
- Soft delete support (@DeleteDateColumn)
- Repository with custom query methods
- Migration (typeorm migration:generate)
- Seeder
- TypeScript strict types
```

### 4.2 Prisma integration

```text
@codebase

Set up Prisma for [entity/feature]:

Requirements:
- Prisma schema (models, relations, enums)
- PrismaService (extends PrismaClient, implements OnModuleInit)
- Repository/Service using PrismaService
- Transactions for complex operations
- Pagination helpers
- Migration (prisma migrate dev)
- Seed script
- Type-safe queries (Prisma.XxxWhereInput, etc.)
```

### 4.3 Drizzle ORM integration

```text
@codebase

Set up Drizzle ORM for [entity/feature]:

Requirements:
- Schema definition (drizzle-orm/pg-core or mysql-core)
- DrizzleModule service
- Type-safe queries with Drizzle query builder
- Relations
- Migrations (drizzle-kit)
- Transactions
- Connection pooling
```

### 4.4 Complex query with pagination

```text
@codebase

Implement a complex query for [mô tả: search, filter, join]:

Requirements:
- Query builder or raw SQL (typed)
- Pagination (offset/limit or cursor-based)
- Sorting (multiple fields)
- Filtering (multiple conditions, operators)
- Search (full-text or LIKE)
- Include related entities
- Count total for pagination metadata
- Type-safe query parameters DTO
```

---

## 5. Authentication & Authorization

### 5.1 JWT Authentication

```text
@codebase

Implement JWT authentication:

Requirements:
- AuthModule with JwtModule configuration
- Login endpoint (email/password → JWT access + refresh tokens)
- Registration endpoint with password hashing (bcrypt)
- JwtAuthGuard (validate access token)
- JwtStrategy (Passport)
- Refresh token endpoint
- Token blacklisting (optional)
- ConfigModule for JWT secrets
- Types for JWT payload
```

### 5.2 Role-based Authorization

```text
@codebase

Implement role-based access control (RBAC):

Requirements:
- Roles enum
- @Roles() decorator
- RolesGuard
- SetMetadata for roles
- Combine with JwtAuthGuard
- Per-endpoint role requirements
- SuperAdmin override (if applicable)
- Type-safe role checking
```

### 5.3 API Key Authentication

```text
@codebase

Implement API key authentication for [service-to-service / external clients]:

Requirements:
- ApiKeyGuard
- API key storage (database, not hardcoded)
- API key generation and rotation
- Rate limiting per API key
- Scoped permissions per key
- @Public() decorator to skip auth
- Header-based: X-API-Key
```

### 5.4 OAuth2 / Social Login

```text
@codebase

Implement OAuth2 login with [Google / GitHub / Facebook]:

Requirements:
- Passport strategy for provider
- Callback endpoint
- User creation/linking
- JWT token generation after OAuth
- Frontend redirect flow
- Error handling (denied, account exists)
- TypeScript types for provider profile
```

---

## 6. Advanced Patterns

### 6.1 CQRS + Event Sourcing

```text
@codebase

Implement CQRS for [feature] using @nestjs/cqrs:

Requirements:
- Commands: Create/Update/Delete with handlers
- Queries: Get/List with handlers
- Events: domain events with handlers
- Event bus for event propagation
- Aggregate root (if event sourcing)
- Saga for orchestrating multi-step flows
- TypeScript types for all commands/queries/events
```

### 6.2 Microservices communication

```text
@codebase

Set up microservice communication for [feature]:

Requirements:
- Transport: [TCP / Redis / RabbitMQ / Kafka / gRPC]
- Message patterns (@MessagePattern for request/response)
- Event patterns (@EventPattern for event-driven)
- ClientProxy for sending messages
- Serialization/deserialization
- Error handling across services
- Health checks
- TypeScript types for messages
```

### 6.3 Task Scheduling

```text
@codebase

Create scheduled tasks with @nestjs/schedule:

Requirements:
- @Cron() for cron-based scheduling
- @Interval() for interval-based
- @Timeout() for one-time delayed execution
- Proper error handling (don't crash the app)
- Logging
- Lock mechanism for clustered environments
- Enable/disable via configuration
- Health monitoring for scheduled tasks
```

### 6.4 WebSocket Gateway

```text
@codebase

Create WebSocket gateway for [mô tả: real-time chat, notifications, live dashboard]:

Requirements:
- @WebSocketGateway() with namespace
- @SubscribeMessage() handlers
- Room management (join/leave)
- Authentication (WsGuard)
- Message validation (WsPipe)
- Connection/disconnection lifecycle
- Broadcasting patterns
- TypeScript types for messages
- Client-side integration example
```

### 6.5 Queue processing (BullMQ)

```text
@codebase

Set up BullMQ queue for [mô tả: email, image processing, data sync]:

Requirements:
- @nestjs/bullmq module registration
- Producer service (add jobs to queue)
- Consumer/Processor (@Processor, @WorkerHost)
- Job options (retries, delay, priority, backoff)
- Failed job handling
- Job events (completed, failed, progress)
- Dashboard (bull-board) if needed
- TypeScript types for job data
- Graceful shutdown
```

### 6.6 Caching

```text
@codebase

Implement caching for [feature/endpoints]:

Requirements:
- @nestjs/cache-manager with [Redis / in-memory]
- @CacheKey() and @CacheTTL() decorators
- CacheInterceptor for automatic caching
- Manual cache operations (get, set, del) in service
- Cache invalidation strategy
- Cache key generation strategy
- Serialization handling
```

---

## 7. Debug Prompts

### 7.1 Dependency Injection Error

```text
@terminal
@codebase

NestJS DI error:
[paste: "Nest can't resolve dependencies of...", circular dependency, etc.]

Please:
- Identify the missing or circular dependency
- Check module imports/exports
- Fix provider registration
- Use @Inject() with token if needed
- Use forwardRef() for circular deps (or refactor to avoid)
```

### 7.2 Validation Error (DTO / Pipe)

```text
@terminal
@file [dto file]

Validation not working / wrong validation errors:
[mô tả]

Please:
- Check class-validator decorators on DTO
- Verify ValidationPipe is registered globally
- Check whitelist/transform options
- Verify DTO class (not interface – decorators need class)
- Check nested validation (@ValidateNested + @Type)
```

### 7.3 Guard / Middleware not executing

```text
@codebase

Guard/middleware not executing for [endpoint]:
[mô tả: 403 when should pass, or no auth check]

Please:
- Check guard registration (global, controller, method)
- Check execution order (middleware → guard → interceptor → pipe)
- Verify @UseGuards() decorator placement
- Check @Public() / @SkipAuth() decorator if exists
- Verify module imports
```

### 7.4 Database / ORM Error

```text
@terminal
@codebase

Database error:
[paste: TypeORM/Prisma error, migration error, query error]

Please:
- Identify the database issue
- Fix entity/schema definition
- Fix query
- Check connection configuration
- Fix migration if needed
```

### 7.5 Memory Leak / Performance

```text
@codebase

NestJS performance issue:
[mô tả: memory growing, slow endpoints, high CPU]

Please:
- Check for: unclosed connections, event listener leaks, cache unbounded growth
- Check provider scopes (request-scoped providers can cause issues)
- Check database query efficiency (N+1, missing indexes)
- Check for synchronous heavy computation (move to queue)
- Suggest profiling approach
```

---

## 8. Testing Prompts

### 8.1 Unit tests

```text
@codebase

Write unit tests for @file [filepath].

Requirements:
- Use NestJS Testing module (Test.createTestingModule)
- Mock all dependencies
- Test each method
- Test error cases and edge cases
- Test guard/pipe/interceptor logic
- Proper beforeEach setup
- TypeScript types for mocks
```

### 8.2 Integration tests (e2e)

```text
@codebase

Write e2e tests for [module/endpoint]:

Requirements:
- Use NestJS INestApplication with supertest
- Full module with real (test) database
- Test complete request/response cycle
- Test authentication flow
- Test validation errors (400/422)
- Test authorization (403)
- Test not found (404)
- Database cleanup between tests
- Test concurrent requests if relevant
```

### 8.3 Testing guards & interceptors

```text
@codebase

Write tests for [Guard / Interceptor / Pipe]:

Requirements:
- Create mock ExecutionContext
- Test with different scenarios (authenticated/unauthenticated, valid/invalid)
- Test edge cases
- Verify correct exceptions thrown
- Verify data transformation (interceptors/pipes)
```

---

## 9. Best Practices & Anti-patterns

### ✅ DO

- Dùng modular architecture (1 module per feature)
- Dùng DTOs với class-validator cho mọi input
- Dùng Guards cho auth, Pipes cho validation, Interceptors cho transform
- Dùng ConfigModule + .env cho configuration
- Dùng global ValidationPipe với `whitelist: true, transform: true`
- Dùng custom exceptions kế thừa HttpException
- Dùng Logger (NestJS built-in) thay vì console.log
- Dùng interfaces cho injectable services
- Tách business logic (Service) khỏi HTTP handling (Controller)
- Dùng health checks (@nestjs/terminus)
- Dùng Swagger (@nestjs/swagger) cho API documentation
- Dùng proper HTTP status codes

### ❌ DON'T

- Không đặt business logic trong Controllers
- Không dùng `any` type
- Không expose entity trực tiếp trong response (dùng Response DTO)
- Không hardcode config values (dùng ConfigService)
- Không dùng synchronous operations cho I/O
- Không skip validation (luôn validate input)
- Không dùng request-scoped providers trừ khi thực sự cần
- Không dùng circular dependencies (refactor thay vì forwardRef)
- Không dùng `@Res()` decorator (mất NestJS interceptors/serialization)
- Không commit `.env` file
- Không skip error handling – mọi endpoint phải handle errors
- Không dùng `console.log` – dùng NestJS Logger

### Ví dụ: Clean NestJS Service

```typescript
import {
  Injectable,
  NotFoundException,
  Logger,
} from '@nestjs/common';
import { InjectRepository } from '@nestjs/typeorm';
import { Repository } from 'typeorm';
import { User } from './entities/user.entity';
import { CreateUserDto } from './dto/create-user.dto';
import { UpdateUserDto } from './dto/update-user.dto';
import { UserResponseDto } from './dto/user-response.dto';
import { PaginationDto } from '../common/dto/pagination.dto';
import { PaginatedResult } from '../common/interfaces/paginated-result.interface';

@Injectable()
export class UsersService {
  private readonly logger = new Logger(UsersService.name);

  constructor(
    @InjectRepository(User)
    private readonly userRepo: Repository<User>,
  ) {}

  async create(dto: CreateUserDto): Promise<UserResponseDto> {
    this.logger.log(`Creating user: ${dto.email}`);

    const user = this.userRepo.create(dto);
    const saved = await this.userRepo.save(user);

    this.logger.log(`User created: ${saved.id}`);
    return UserResponseDto.fromEntity(saved);
  }

  async findAll(pagination: PaginationDto): Promise<PaginatedResult<UserResponseDto>> {
    const [users, total] = await this.userRepo.findAndCount({
      skip: pagination.skip,
      take: pagination.limit,
      order: { createdAt: 'DESC' },
    });

    return {
      data: users.map(UserResponseDto.fromEntity),
      total,
      page: pagination.page,
      limit: pagination.limit,
    };
  }

  async findOne(id: string): Promise<UserResponseDto> {
    const user = await this.userRepo.findOne({ where: { id } });
    if (!user) {
      throw new NotFoundException(`User ${id} not found`);
    }
    return UserResponseDto.fromEntity(user);
  }

  async update(id: string, dto: UpdateUserDto): Promise<UserResponseDto> {
    const user = await this.userRepo.findOne({ where: { id } });
    if (!user) {
      throw new NotFoundException(`User ${id} not found`);
    }

    Object.assign(user, dto);
    const updated = await this.userRepo.save(user);

    this.logger.log(`User updated: ${id}`);
    return UserResponseDto.fromEntity(updated);
  }

  async remove(id: string): Promise<void> {
    const result = await this.userRepo.softDelete(id);
    if (result.affected === 0) {
      throw new NotFoundException(`User ${id} not found`);
    }
    this.logger.log(`User deleted: ${id}`);
  }
}
```

---

> 📌 Quay lại [JavaScript](./README.md) | [Handbook chính](../cursor-ide-handbook.md) | Xem thêm: [Next.js](./nextjs.md) | [Angular](./angular.md)
