# 📘 CURSOR PROMPTS – JavaScript / TypeScript

**Rules • Prompts • Debug • Best Practices cho hệ sinh thái JavaScript trong Cursor IDE**

> Thư mục này là phần mở rộng của [cursor-ide-handbook.md](../cursor-ide-handbook.md) – Section 11.

---

## Cấu trúc thư mục

```
javascript/
├── README.md              ← Bạn đang ở đây (JS/TS core)
├── react.md               ← React + React ecosystem
├── nextjs.md              ← Next.js (App Router + Pages Router)
├── nestjs.md              ← NestJS (Node.js backend framework)
├── angular.md             ← Angular (full framework)
└── vue.md                 ← Vue.js 3 + Nuxt
```

---

## Mục lục (file này)

- [1. Rules template cho JavaScript / TypeScript](#1-rules-template-cho-javascript--typescript)
- [2. Prompt Library cho JS/TS](#2-prompt-library-cho-jsts)
- [3. Node.js Backend Prompts](#3-nodejs-backend-prompts)
- [4. Debug Prompts](#4-debug-prompts)
- [5. Testing Prompts](#5-testing-prompts)
- [6. Tooling & Build](#6-tooling--build)
- [7. Best Practices & Anti-patterns](#7-best-practices--anti-patterns)

---

## 1. Rules template cho JavaScript / TypeScript

### 1.1 File `.cursorrules` cho JS/TS project

```txt
You are an expert JavaScript/TypeScript developer with deep knowledge
of modern ES2024+, Node.js, and the JavaScript ecosystem.

# General JS/TS Rules
- Write modern JavaScript (ES2024+) or TypeScript (5.x).
- Follow existing project coding style and conventions.
- Prefer TypeScript over JavaScript when the project supports it.
- Prefer readability and simplicity over cleverness.
- Do NOT introduce new npm packages unless explicitly asked.
- Keep bundle size in mind – avoid large dependencies for small tasks.

# TypeScript Strict Rules
- Enable strict mode in tsconfig.json.
- No 'any' type – use explicit types, generics, or 'unknown'.
- Prefer 'interface' over 'type' for object shapes.
- Use 'type' for unions, intersections, and utility types.
- Use 'as const' for literal types.
- Use discriminated unions for state management.
- Use 'satisfies' operator for type checking without widening.
- Use 'readonly' for immutable data.

# Naming Conventions
- camelCase for: variables, functions, methods, properties.
- PascalCase for: classes, interfaces, types, enums, React components.
- UPPER_SNAKE_CASE for: constants, env variables.
- kebab-case for: file names (or follow project convention).
- Prefix interfaces with nothing (not "I" – that's C# style).

# Functions & Modules
- Prefer arrow functions for callbacks and short functions.
- Use named function declarations for top-level functions.
- Prefer named exports over default exports.
- Keep functions small (< 30 lines) and focused.
- Use early returns to reduce nesting.

# Async
- Use async/await instead of .then()/.catch() chains.
- Always handle errors in async functions (try/catch or .catch()).
- Use Promise.all() / Promise.allSettled() for concurrent operations.
- Never use callbacks when Promises are available.
- Handle AbortController for cancellation.

# Error Handling
- Use custom Error classes for domain-specific errors.
- Always catch specific errors when possible.
- Never swallow errors silently (empty catch blocks).
- Log errors with context (structured logging).
- Use Result pattern (ok/err) for expected failures.

# Security
- Never use eval() or Function() constructor with user input.
- Sanitize user input before rendering (prevent XSS).
- Use parameterized queries for database operations.
- Validate input with zod/joi/yup.
- Never expose secrets in client-side code.

# Output Format
- Always start with: "Files changed/created:"
- List each file path clearly.
- Explain briefly what was changed and why.
- Use code blocks with 'typescript' or 'javascript' language identifier.
```

### 1.2 File `.cursor/rules/typescript.mdc`

```
---
description: Rules for TypeScript/JavaScript files
globs: *.ts, *.tsx, *.js, *.jsx, *.mjs, *.mts
---

- Write modern TypeScript (5.x) with strict mode
- No 'any' type – use explicit types, generics, or 'unknown'
- Prefer 'interface' for object shapes, 'type' for unions/intersections
- Use async/await, not .then()/.catch() chains
- Use named exports over default exports
- Use arrow functions for callbacks, named functions for top-level
- Handle all errors (no empty catch blocks)
- Use early returns to reduce nesting
- Validate input with zod/joi at API boundaries
- Follow existing naming conventions (camelCase functions, PascalCase types)
- Keep functions < 30 lines, single responsibility
- Use 'readonly' and 'as const' for immutable data
```

---

## 2. Prompt Library cho JS/TS

### 2.1 Tạo module mới

```text
@codebase

Create a TypeScript module for [mô tả chức năng].

Requirements:
- Proper type definitions (interfaces, types)
- Named exports
- Error handling with custom error types
- JSDoc comments for public functions
- Async/await for I/O operations
- Input validation (zod if project uses it)
- Example usage in comments

Follow existing project structure.
```

### 2.2 Tạo utility functions

```text
@codebase

Create utility functions for [mô tả: date formatting, string helpers, array manipulation, etc.].

Requirements:
- Pure functions (no side effects)
- Full TypeScript generics where applicable
- Overloaded signatures if multiple input types
- Edge case handling (null, undefined, empty)
- JSDoc with @example
- Unit tests
- Tree-shakeable (named exports)
```

### 2.3 Tạo TypeScript types/interfaces

```text
@codebase

Create TypeScript type definitions for [mô tả domain/API].

Requirements:
- Interfaces for object shapes
- Discriminated unions for state/variants
- Utility types (Pick, Omit, Partial, Required) where useful
- Generic types for reusable patterns
- Branded types for IDs (UserId, OrderId)
- Zod schemas + inferred types (if project uses zod)
- Export from types.ts or index.ts
```

### 2.4 Refactor JS/TS code

```text
@file [filepath]

Refactor this code:
- Convert JavaScript to TypeScript (add types)
- Replace var with const/let
- Replace .then()/.catch() with async/await
- Replace function() with arrow functions where appropriate
- Use destructuring
- Use optional chaining (?.) and nullish coalescing (??)
- Extract magic strings/numbers to constants
- Use template literals instead of string concatenation
- Add error handling
- Split large functions into smaller ones

Keep exact same behavior.
```

### 2.5 Type-safe API client

```text
@codebase

Create a type-safe API client for [API description / OpenAPI spec].

Requirements:
- Fetch-based or axios-based (follow project)
- Generic request/response types
- Error handling with typed errors
- Request/response interceptors
- AbortController support for cancellation
- Retry logic for transient failures
- Base URL configuration
- Auth token injection
- Type inference for endpoints
```

### 2.6 Validation schemas

```text
@codebase

Create validation schemas for [entity/feature] using [zod / joi / yup].

Requirements:
- Schema for each input (create, update, query params)
- Proper error messages (custom per field)
- Type inference from schema (z.infer<typeof schema>)
- Reusable base schemas with extend/merge
- Transform/preprocess for data coercion
- Test edge cases
```

---

## 3. Node.js Backend Prompts

### 3.1 Express.js API

```text
@codebase

Create Express.js API endpoints for [resource]:

Requirements:
- Router with proper routes
- Controller with typed req/res
- Middleware: auth, validation, error handling
- Input validation (zod/joi)
- Proper HTTP status codes
- Error handling middleware
- TypeScript types for request/response
- Async error wrapper

Follow existing Express patterns.
```

### 3.2 Database module (Prisma / Drizzle / TypeORM)

```text
@codebase

Create database layer for [entity] using [Prisma / Drizzle / TypeORM].

Requirements:
- Schema/model definition
- Migration
- CRUD repository/service
- Relationships
- Transactions for multi-step operations
- Type-safe queries
- Pagination and filtering
- Seeder with test data
```

### 3.3 WebSocket server

```text
@codebase

Create WebSocket server for [mô tả: real-time chat, notifications, live data].

Requirements:
- Use [ws / socket.io / @fastify/websocket]
- Connection management (connect, disconnect, heartbeat)
- Room/channel support
- Message validation and typing
- Authentication
- Graceful shutdown
- Error handling and reconnection logic
- Rate limiting
```

### 3.4 Background job / Queue

```text
@codebase

Create background job processing for [mô tả].

Requirements:
- Use [BullMQ / Agenda / custom]
- Job definition with TypeScript types
- Retry logic with backoff
- Dead letter queue for failed jobs
- Concurrency control
- Progress tracking
- Graceful shutdown
- Logging and monitoring
```

### 3.5 Middleware chain

```text
@codebase

Create middleware for [mô tả: auth, rate limiting, logging, CORS, request ID].

Requirements:
- TypeScript-typed middleware
- Proper next() / error handling
- Configuration options
- Unit testable
- Works with [Express / Fastify / Koa]
- Documentation

Register in proper order.
```

---

## 4. Debug Prompts

### 4.1 Runtime Error

```text
@terminal
@file [filepath]

JavaScript/TypeScript error:
[paste full error + stack trace]

Please:
- Identify root cause
- Fix the bug
- Add proper error handling
- Explain what went wrong
```

### 4.2 TypeScript Compilation Error

```text
@terminal
@file [filepath]

TypeScript error:
[paste tsc error output]

Please:
- Explain the type error in plain language
- Fix the type issue (don't use 'any' as escape hatch)
- If the types are too complex, suggest a cleaner type design
```

### 4.3 Async / Promise Bug

```text
@file [filepath]

Async bug:
[mô tả: unhandled rejection, race condition, deadlock, memory leak, infinite loop]

Please:
- Identify the async issue
- Fix with proper async/await patterns
- Add error handling
- Check for: missing await, unhandled promise, concurrent mutation
```

### 4.4 Memory Leak (Node.js)

```text
@terminal
@codebase

Node.js memory leak:
[mô tả: heap growing, OOM crash, high RSS]

Heap snapshot (if available):
[paste top retainers or describe]

Please:
- Identify leak source (event listeners, closures, caches, streams)
- Fix the leak
- Add cleanup logic
- Suggest monitoring approach
```

### 4.5 Build / Bundle Error

```text
@terminal
@codebase

Build error:
[paste full error from webpack / vite / esbuild / tsc / turbo]

Please:
- Identify the build issue
- Fix configuration or source code
- Check for: missing module, circular dependency, config error
```

### 4.6 Package / Dependency Issue

```text
@terminal

npm/yarn/pnpm error:
[paste full error output]

Please:
- Diagnose the dependency issue
- Show exact commands to fix
- Update package.json if needed
- Check for peer dependency conflicts
```

---

## 5. Testing Prompts

### 5.1 Unit tests (Vitest / Jest)

```text
@codebase

Write unit tests for @file [filepath].

Requirements:
- Framework: [Vitest / Jest]
- describe/it blocks with clear names
- Test each exported function
- Happy path + edge cases + error cases
- Mock dependencies (vi.mock / jest.mock)
- Type-safe mocks
- beforeEach/afterEach for setup
- expect() with specific matchers
```

### 5.2 API Integration tests

```text
@codebase

Write API integration tests for [endpoint/module].

Requirements:
- Use supertest (Express) or app.inject (Fastify)
- Test full request/response cycle
- Test status codes, headers, body
- Test authentication/authorization
- Test validation errors
- Database setup/teardown
- Typed test helpers
```

### 5.3 E2E tests (Playwright / Cypress)

```text
@codebase

Write E2E tests for [page/flow].

Requirements:
- Framework: [Playwright / Cypress]
- Test user journey: [mô tả flow]
- Page Object Model (or similar abstraction)
- Wait for async elements properly
- Test responsive design if needed
- Screenshot/video on failure
- CI-friendly configuration
```

### 5.4 Test fixtures & factories

```text
@codebase

Create test fixtures/factories for [entities].

Requirements:
- Type-safe factory functions
- Overridable defaults
- Relationship handling
- Database integration (create in DB or in-memory)
- Faker for realistic data
- Reusable across unit and integration tests
```

---

## 6. Tooling & Build

### 6.1 tsconfig.json

```text
@codebase

Create/update tsconfig.json for this project.

Requirements:
- Strict mode enabled
- Target: [ES2022 / ESNext]
- Module: [NodeNext / ESNext / CommonJS]
- Path aliases (@/ for src/)
- Separate configs: tsconfig.json (base), tsconfig.build.json, tsconfig.test.json
- Include/exclude correct paths
- Declaration files (.d.ts) generation if library
```

### 6.2 ESLint + Prettier

```text
@codebase

Set up ESLint + Prettier for this project.

Requirements:
- ESLint flat config (eslint.config.js)
- TypeScript ESLint parser
- Recommended rules + strict type-checking
- Prettier integration (eslint-config-prettier)
- Import sorting (eslint-plugin-import)
- Framework-specific rules: [React/Vue/Angular/Node]
- .prettierrc with team preferences
- Scripts in package.json: lint, lint:fix, format
```

### 6.3 Monorepo setup

```text
@codebase

Set up monorepo with [Turborepo / Nx / pnpm workspaces].

Requirements:
- Workspace packages: [list packages]
- Shared configs (tsconfig, eslint, prettier)
- Shared packages (types, utils, ui)
- Build pipeline with proper dependencies
- Dev mode with watch
- Cached builds
- CI configuration
```

---

## 7. Best Practices & Anti-patterns

### ✅ DO (Best Practices)

- Dùng TypeScript strict mode cho mọi project
- Dùng `const` by default, `let` khi cần reassign, **không bao giờ** `var`
- Dùng `async/await` thay vì `.then()/.catch()`
- Dùng optional chaining `?.` và nullish coalescing `??`
- Dùng `Array.map/filter/reduce` thay vì for loops khi phù hợp
- Dùng `structuredClone()` cho deep copy
- Dùng `AbortController` cho cancellable operations
- Dùng `using` keyword (TC39 stage 3) cho resource cleanup
- Dùng named exports thay vì default exports
- Dùng path aliases (`@/`) thay vì relative paths dài
- Validate input tại API boundaries (zod/joi)
- Handle tất cả Promise rejections

### ❌ DON'T (Anti-patterns)

- Không dùng `any` – dùng `unknown` + type narrowing
- Không dùng `eval()` hoặc `new Function()` với user input
- Không dùng `==` – luôn dùng `===`
- Không dùng `var` – dùng `const` / `let`
- Không mutate function arguments
- Không dùng `@ts-ignore` – dùng `@ts-expect-error` nếu bắt buộc
- Không dùng `Number()` cho user input – validate trước
- Không dùng `delete` operator cho performance-critical objects
- Không dùng `for...in` cho arrays – dùng `for...of` hoặc methods
- Không dùng callback hell – dùng async/await
- Không commit `node_modules/` hoặc `.env`
- Không dùng `console.log` trong production – dùng logger

### Ví dụ: Type-safe Error Handling

```typescript
// Custom error hierarchy
class AppError extends Error {
  constructor(
    message: string,
    public readonly code: string,
    public readonly statusCode: number = 500,
    public readonly isOperational: boolean = true,
  ) {
    super(message);
    this.name = this.constructor.name;
  }
}

class NotFoundError extends AppError {
  constructor(resource: string, id: string | number) {
    super(`${resource} with id ${id} not found`, 'NOT_FOUND', 404);
  }
}

class ValidationError extends AppError {
  constructor(
    message: string,
    public readonly errors: Record<string, string[]>,
  ) {
    super(message, 'VALIDATION_ERROR', 422);
  }
}

// Result pattern
type Result<T, E = AppError> =
  | { ok: true; value: T }
  | { ok: false; error: E };

function ok<T>(value: T): Result<T, never> {
  return { ok: true, value };
}

function err<E>(error: E): Result<never, E> {
  return { ok: false, error };
}

// Usage
async function getUser(id: string): Promise<Result<User, NotFoundError>> {
  const user = await db.users.findUnique({ where: { id } });
  if (!user) return err(new NotFoundError('User', id));
  return ok(user);
}
```

---

## Xem thêm: Framework-specific prompts

| Framework | File | Nội dung chính |
|-----------|------|---------------|
| **React** | [react.md](./react.md) | Components, hooks, state management, React 19, RSC |
| **Next.js** | [nextjs.md](./nextjs.md) | App Router, Server Components, API Routes, SSR/SSG/ISR |
| **NestJS** | [nestjs.md](./nestjs.md) | Controllers, Services, Modules, Guards, Pipes, TypeORM/Prisma |
| **Angular** | [angular.md](./angular.md) | Components, Services, RxJS, Signals, Standalone, NgRx |
| **Vue** | [vue.md](./vue.md) | Composition API, Pinia, Vue Router, Nuxt 3 |

---

> 📌 Quay lại [Handbook chính](../cursor-ide-handbook.md)
