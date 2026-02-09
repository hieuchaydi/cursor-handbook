# 📘 CURSOR PROMPTS – Angular

**Rules • Prompts • Debug • Best Practices cho Angular trong Cursor IDE**

> File này thuộc thư mục [javascript/](./README.md) – hệ sinh thái JS/TS.

---

## Mục lục

- [1. Rules template cho Angular](#1-rules-template-cho-angular)
- [2. Component Prompts](#2-component-prompts)
- [3. Services & State Management](#3-services--state-management)
- [4. Forms & Validation](#4-forms--validation)
- [5. Routing & Navigation](#5-routing--navigation)
- [6. HTTP & API Integration](#6-http--api-integration)
- [7. Debug Prompts](#7-debug-prompts)
- [8. Testing Prompts](#8-testing-prompts)
- [9. Best Practices & Anti-patterns](#9-best-practices--anti-patterns)

---

## 1. Rules template cho Angular

### 1.1 File `.cursor/rules/angular.mdc`

```
---
description: Rules for Angular component and service files
globs: *.component.ts, *.service.ts, *.module.ts, *.directive.ts, *.pipe.ts, *.guard.ts, *.interceptor.ts, *.resolver.ts
---

- Use Angular 17+ features: standalone components, signals, control flow (@if, @for, @switch)
- Prefer standalone components over NgModule-declared components
- Use signals for reactive state (signal, computed, effect)
- Use inject() function over constructor injection
- Use TypeScript strict mode, no 'any'
- Follow Angular naming conventions: feature.type.ts (user.component.ts, user.service.ts)
- One component per file
- Use OnPush change detection by default
- Proper cleanup: use DestroyRef or takeUntilDestroyed()
- Smart/Container components fetch data, Dumb/Presentational components receive @Input
- Use Angular CDK for complex UI patterns
- Lazy load routes and heavy feature modules
- Handle loading, error, and empty states in templates
```

### 1.2 Full `.cursorrules` cho Angular project

```txt
You are an expert Angular developer with deep knowledge of Angular 17+,
TypeScript, RxJS, Signals, and enterprise frontend architecture.

# Angular Architecture
- Use standalone components (no NgModule unless legacy).
- Smart (container) + Dumb (presentational) component pattern.
- Feature-based folder structure.
- Services for business logic, Components for UI.
- Signals for synchronous reactive state.
- RxJS for async streams (HTTP, WebSocket, events).

# Components
- Standalone components by default.
- OnPush change detection strategy.
- Use signals: signal(), computed(), effect().
- Use new control flow: @if, @for, @switch, @defer.
- Use inject() function (not constructor injection for new code).
- Keep templates small, extract reusable parts.
- @Input() with required where applicable.
- @Output() with EventEmitter for child-to-parent.
- input() and output() signal-based alternatives (Angular 17.1+).

# State Management
- Simple state: signals in services.
- Complex state: NgRx Store / NgRx SignalStore.
- Server state: HTTP + signals/RxJS.
- Avoid mutable state, prefer immutable updates.

# RxJS
- Use operators correctly (switchMap, mergeMap, concatMap, exhaustMap).
- Always unsubscribe: takeUntilDestroyed(), DestroyRef, async pipe.
- No nested subscribes – use operators to compose.
- Use shareReplay for shared observables.

# TypeScript
- Strict mode. No 'any'.
- Interfaces for data shapes, types for unions.
- Enums for fixed sets.
- Strong typing for HTTP responses.

# Output Format
- Always start with: "Files changed/created:"
- List each file path clearly.
- Specify standalone vs NgModule if relevant.
```

---

## 2. Component Prompts

### 2.1 Tạo standalone component

```text
@codebase

Create a standalone Angular component for [mô tả UI/chức năng].

Requirements:
- standalone: true
- changeDetection: OnPush
- TypeScript strict types for inputs/outputs
- Use signals for local state
- Use new control flow (@if, @for, @switch)
- @defer for heavy sub-components (if applicable)
- Handle states: loading, error, empty, success
- Responsive design
- Accessible (aria labels, keyboard navigation)
- Styled with [SCSS / Tailwind] following project convention
- Input/Output with proper types

Follow existing component patterns.
```

### 2.2 Tạo presentational (dumb) component

```text
@codebase

Create a presentational component for [mô tả: card, list item, badge, avatar, etc.].

Requirements:
- standalone: true, OnPush
- @Input() / input() for all data (no service injection)
- @Output() / output() for events
- No business logic – pure UI
- Required inputs where appropriate
- Template-driven rendering only
- Reusable across features
- Storybook-ready (if project uses Storybook)
```

### 2.3 Tạo smart (container) component

```text
@codebase

Create a smart/container component for [mô tả feature page]:

Requirements:
- standalone: true, OnPush
- Inject services for data fetching
- Use signals or RxJS for reactive data
- Delegate UI to presentational child components
- Handle loading/error states
- Route params/query handling if needed
- Cleanup subscriptions (DestroyRef / takeUntilDestroyed)
```

### 2.4 Tạo directive

```text
@codebase

Create a standalone directive for [mô tả: tooltip, click-outside, auto-focus, highlight, lazy-load].

Requirements:
- standalone: true
- @Directive with proper selector ([appFeatureName])
- Use inject() for dependencies
- HostListener / HostBinding as needed
- Cleanup on destroy
- Input parameters for configuration
- TypeScript strict types
```

### 2.5 Tạo pipe

```text
@codebase

Create a standalone pipe for [mô tả: date format, currency, truncate, filter, sort].

Requirements:
- standalone: true, pure: true (or false if needs change detection)
- Transform method with proper types
- Handle null/undefined input
- Parameterized if needed
- Unit test
```

### 2.6 Refactor to standalone + signals

```text
@file [filepath]

Refactor this Angular component to modern patterns:
- Convert to standalone component (remove NgModule dependency)
- Replace constructor injection with inject()
- Replace imperative state with signals (signal, computed)
- Replace *ngIf/*ngFor with @if/@for control flow
- Add OnPush change detection
- Replace manual subscribe with toSignal() or async pipe
- Use takeUntilDestroyed() for cleanup
- Add proper TypeScript types

Keep exact same behavior.
```

---

## 3. Services & State Management

### 3.1 Tạo service

```text
@codebase

Create an Angular service for [mô tả business logic].

Requirements:
- @Injectable({ providedIn: 'root' }) (or feature-scoped)
- Signals for state management
- Typed methods
- Error handling
- Logging
- Unit testable (injectable dependencies)

Follow existing service patterns.
```

### 3.2 Signal-based state service

```text
@codebase

Create a signal-based state service for [feature]:

Requirements:
- Private WritableSignal for state
- Public readonly Signal/computed for derived state
- Methods to update state (immutable updates)
- Loading/error signals
- Async operations with proper state transitions
- TypeScript interface for state shape
- Select specific parts of state (computed)
```

### 3.3 NgRx Store

```text
@codebase

Create NgRx store for [feature]:

Requirements:
- Actions: load, loadSuccess, loadFailure, create, update, delete
- Reducer with createReducer/on
- Effects for async operations
- Selectors (createSelector, memoized)
- Feature state interface
- Register with StoreModule.forFeature or provideState
- Entity adapter for collections (if applicable)
- TypeScript strict types
```

### 3.4 NgRx SignalStore

```text
@codebase

Create NgRx SignalStore for [feature]:

Requirements:
- signalStore() with typed state
- withState() for state definition
- withComputed() for derived state
- withMethods() for actions
- withHooks() for lifecycle
- rxMethod() for RxJS integration
- patchState() for immutable updates
- Entity management with withEntities()
```

### 3.5 RxJS patterns

```text
@codebase

Implement RxJS-based data flow for [mô tả: search, polling, real-time, cascading dropdowns].

Requirements:
- Proper operator choice (switchMap vs mergeMap vs concatMap vs exhaustMap)
- Error handling (catchError, retry)
- Loading state
- Debounce for user input
- Cancel previous request (switchMap)
- Share subscription (shareReplay)
- Cleanup (takeUntilDestroyed or unsubscribe)
- Type safety throughout the stream
```

---

## 4. Forms & Validation

### 4.1 Reactive Form

```text
@codebase

Create a reactive form for [mô tả: registration, settings, product creation].

Requirements:
- FormGroup with typed FormControl (FormGroup<{...}>)
- Validators (required, email, minLength, custom validators)
- Custom async validator if needed
- Error messages per field
- Form-level and field-level validation
- Disable submit when invalid
- Submit handler with loading state
- Reset on success
- Accessible form labels and error messages
```

### 4.2 Dynamic form

```text
@codebase

Create a dynamic form for [mô tả: configurable fields, JSON-driven form]:

Requirements:
- FormArray for repeatable fields
- Add/remove field groups dynamically
- Validation per dynamic field
- Type-safe form value extraction
- Nested form groups if needed
- Template rendering based on field config
```

### 4.3 Custom form control

```text
@codebase

Create a custom form control component for [mô tả: date picker, tag input, rich text, rating].

Requirements:
- Implement ControlValueAccessor
- standalone: true
- Works with Reactive Forms and ngModel
- Validation support (NG_VALIDATORS)
- Disabled state handling
- Proper styling integration
- Accessible (ARIA)
- Error state display
```

### 4.4 Multi-step form

```text
@codebase

Create a multi-step form wizard for [mô tả flow]:

Requirements:
- Step components with individual FormGroups
- Navigation (next, back, submit)
- Per-step validation (can't proceed if invalid)
- Progress indicator
- Summary/review step
- Persist state across steps (shared service or parent FormGroup)
- Accessible step announcements
```

---

## 5. Routing & Navigation

### 5.1 Feature routes

```text
@codebase

Create routes for [feature]:

Requirements:
- Standalone route configuration (Routes array)
- Lazy loading with loadComponent / loadChildren
- Route guards (canActivate, canDeactivate, canMatch)
- Route resolvers for data prefetching
- Nested routes / child routes if needed
- Route parameters and query parameters typed
- Title strategy
- Register in app.routes.ts
```

### 5.2 Route Guard

```text
@codebase

Create a route guard for [mô tả: auth check, role check, unsaved changes, feature flag].

Requirements:
- Functional guard (canActivateFn / canDeactivateFn / canMatchFn)
- Use inject() for dependencies
- Return boolean | UrlTree | Observable<boolean>
- Redirect to appropriate page if blocked
- Type-safe route data access
```

### 5.3 Route Resolver

```text
@codebase

Create a route resolver for [mô tả: prefetch data before page renders].

Requirements:
- ResolveFn or class-based Resolver
- Fetch data from service
- Error handling (redirect to error page or return empty)
- Loading state coordination
- Type-safe resolved data access in component
```

---

## 6. HTTP & API Integration

### 6.1 HTTP service

```text
@codebase

Create an HTTP service for [API resource]:

Requirements:
- HttpClient injection
- Typed request/response methods
- Interceptors for: auth token, error handling, logging
- Error handling with typed error responses
- Loading state management
- Retry logic for transient failures
- Cancel previous requests (switchMap)
- Pagination support
```

### 6.2 HTTP Interceptor

```text
@codebase

Create an HTTP interceptor for [mô tả: auth token, error handling, caching, logging, retry].

Requirements:
- Functional interceptor (HttpInterceptorFn)
- provideHttpClient(withInterceptors([...]))
- Proper handling of request/response
- Error handling without breaking the chain
- Skip interceptor for specific requests (if needed)
- TypeScript types
```

### 6.3 API integration layer

```text
@codebase

Create an API integration layer for [external API]:

Requirements:
- Environment-based base URL
- Type-safe endpoint methods
- Request/response DTOs
- Error mapping (API errors → app errors)
- Pagination handling
- Token refresh integration
- Caching strategy
- Offline support (if applicable)
```

---

## 7. Debug Prompts

### 7.1 Change detection issue

```text
@file [filepath]

Component not updating / updating too much:
[mô tả]

Please:
- Check change detection strategy (OnPush?)
- Check if signals/observables are properly used
- Check if objects are mutated (OnPush needs new references)
- Use markForCheck() or signal updates
- Profile with Angular DevTools
```

### 7.2 RxJS subscription leak

```text
@file [filepath]

Suspected subscription leak:
[mô tả: memory growing, multiple subscriptions, repeated HTTP calls]

Please:
- Find subscriptions without cleanup
- Add takeUntilDestroyed() or DestroyRef
- Use async pipe where possible
- Convert to signal with toSignal() where appropriate
- Check for nested subscribes (refactor to operators)
```

### 7.3 Template / Compilation error

```text
@terminal
@file [filepath]

Angular template/compilation error:
[paste error]

Please:
- Fix template syntax error
- Check standalone component imports
- Check input/output bindings
- Verify directive/pipe availability
- Fix TypeScript strict mode errors in template
```

### 7.4 Dependency injection error

```text
@terminal
@codebase

Angular DI error:
[paste: NullInjectorError, No provider for..., circular dependency]

Please:
- Identify missing provider
- Check providedIn configuration
- Check module/standalone imports
- Fix circular dependency
- Verify injection token
```

### 7.5 Routing issue

```text
@codebase

Routing issue:
[mô tả: wrong route, guard blocking, data not resolved, lazy load failing]

Please:
- Check route configuration order (specific before wildcard)
- Check guard logic
- Check resolver error handling
- Verify lazy loading path
- Check route parameter types
```

---

## 8. Testing Prompts

### 8.1 Component tests

```text
@codebase

Write tests for @file [filepath].

Requirements:
- TestBed with standalone component
- Mock services (jasmine spies or jest mocks)
- Test rendering in different states
- Test user interactions (click, input, etc.)
- Test @Input/@Output bindings
- Test async operations (fakeAsync/tick or waitForAsync)
- Accessible assertions
```

### 8.2 Service tests

```text
@codebase

Write tests for service @file [filepath].

Requirements:
- TestBed with service providers
- HttpClientTestingModule for HTTP services
- Mock dependencies
- Test each public method
- Test error handling
- Test RxJS streams (marble testing if complex)
- Test signal state transitions
```

### 8.3 E2E tests (Playwright / Cypress)

```text
@codebase

Write E2E tests for [feature/page]:

Requirements:
- Framework: [Playwright / Cypress]
- Page Object Model
- Test user journeys
- Test routing and navigation
- Test form submission
- Mock API if needed
- Test responsive behavior
- Accessibility checks
```

---

## 9. Best Practices & Anti-patterns

### ✅ DO

- Dùng standalone components (Angular 17+)
- Dùng signals cho reactive state
- Dùng `inject()` function thay vì constructor injection
- Dùng `@if/@for/@switch` control flow thay vì `*ngIf/*ngFor/[ngSwitch]`
- Dùng `@defer` cho lazy-load heavy components
- Dùng OnPush change detection
- Dùng `takeUntilDestroyed()` cho RxJS cleanup
- Dùng `toSignal()` để convert Observable → Signal
- Dùng Smart/Dumb component pattern
- Dùng lazy loading cho route features
- Dùng typed forms (`FormGroup<{...}>`)
- Dùng functional guards, interceptors, resolvers

### ❌ DON'T

- Không subscribe trong component rồi quên unsubscribe
- Không dùng `any` type
- Không dùng nested subscribes – compose với RxJS operators
- Không đặt business logic trong components
- Không dùng NgModule cho new features (dùng standalone)
- Không mutate objects với OnPush (tạo reference mới)
- Không dùng `document.querySelector` – dùng ViewChild/ElementRef
- Không dùng `setTimeout` cho timing – dùng RxJS timer/delay
- Không dùng `ngOnChanges` khi `computed()` signal đủ
- Không dùng `@ViewChild` có `static: true` trừ khi thực sự cần trong `ngOnInit`
- Không skip `trackBy` trong `@for` (performance)
- Không commit environment files với secrets

---

> 📌 Quay lại [JavaScript](./README.md) | [Handbook chính](../cursor-ide-handbook.md) | Xem thêm: [React](./react.md) | [Vue](./vue.md)
