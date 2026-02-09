# 📘 CURSOR PROMPTS – Next.js

**Rules • Prompts • Debug • Best Practices cho Next.js trong Cursor IDE**

> File này thuộc thư mục [javascript/](./README.md) – hệ sinh thái JS/TS.

---

## Mục lục

- [1. Rules template cho Next.js](#1-rules-template-cho-nextjs)
- [2. App Router Prompts](#2-app-router-prompts)
- [3. Server Components & Server Actions](#3-server-components--server-actions)
- [4. Data Fetching & Caching](#4-data-fetching--caching)
- [5. API Routes](#5-api-routes)
- [6. Authentication & Middleware](#6-authentication--middleware)
- [7. Performance & SEO](#7-performance--seo)
- [8. Debug Prompts](#8-debug-prompts)
- [9. Testing Prompts](#9-testing-prompts)
- [10. Best Practices & Anti-patterns](#10-best-practices--anti-patterns)

---

## 1. Rules template cho Next.js

### 1.1 File `.cursor/rules/nextjs.mdc`

```
---
description: Rules for Next.js App Router files
globs: app/**/*.ts, app/**/*.tsx, app/**/layout.tsx, app/**/page.tsx, app/**/loading.tsx, app/**/error.tsx
---

- Use App Router (app/ directory) patterns
- Server Components by default, 'use client' only when needed
- Use Server Actions for form submissions and mutations
- Every route segment should have: page.tsx, layout.tsx (if needed), loading.tsx, error.tsx
- Use generateMetadata() for dynamic SEO metadata
- Use generateStaticParams() for static generation of dynamic routes
- Use proper caching: fetch() with cache/revalidate options
- Colocate components, utils, types with their route segment
- Use 'use server' for Server Actions, 'use client' for Client Components
- Handle loading states with loading.tsx or Suspense boundaries
- Handle errors with error.tsx (client component with 'use client')
- Use next/image for all images (optimization)
- Use next/link for all internal navigation
- Use next/font for font optimization
```

### 1.2 Full `.cursorrules` cho Next.js project

```txt
You are an expert Next.js developer with deep knowledge of Next.js 15,
React 19, App Router, Server Components, Server Actions, and TypeScript.

# Next.js Architecture
- Use App Router (app/ directory). Not Pages Router.
- Server Components by default. Only add 'use client' when needed:
  - Event handlers (onClick, onChange)
  - Browser APIs (window, document)
  - React hooks (useState, useEffect)
  - Third-party client libraries
- Use Server Actions ('use server') for mutations.
- Colocate: components/, hooks/, utils/, types/ within route segments.

# Data Fetching
- Fetch data in Server Components (not in Client Components).
- Use fetch() with proper cache options:
  - cache: 'force-cache' (default, static)
  - cache: 'no-store' (dynamic)
  - next: { revalidate: 60 } (ISR)
- Use React cache() for request deduplication.
- Use generateStaticParams() for pre-rendering dynamic routes.

# File Conventions
- page.tsx: route UI
- layout.tsx: shared layout (preserved across navigations)
- loading.tsx: loading UI (Suspense boundary)
- error.tsx: error UI ('use client' required)
- not-found.tsx: 404 page
- route.ts: API route handler

# Performance
- Use next/image with proper width/height/sizes.
- Use next/font for self-hosted fonts.
- Use next/link with prefetch for navigation.
- Lazy load Client Components below the fold.
- Use streaming with Suspense for progressive rendering.

# TypeScript
- No 'any'. Strict types for all props, params, searchParams.
- Use PageProps type: { params: Promise<{ id: string }>, searchParams: Promise<Record<string, string>> }.
- Type Server Actions input and output.

# Output Format
- Always start with: "Files changed/created:"
- Specify if file is Server Component or Client Component.
- List route structure changes.
```

---

## 2. App Router Prompts

### 2.1 Tạo route segment đầy đủ

```text
@codebase

Create a new route segment for [mô tả page/feature].
Route: /[path]

Required files:
1. page.tsx – Main page component (Server Component)
2. layout.tsx – Layout if needed (navigation, sidebar)
3. loading.tsx – Loading skeleton
4. error.tsx – Error boundary ('use client')
5. not-found.tsx – 404 handling (if dynamic route)

Requirements:
- Server Component by default
- Fetch data in page.tsx
- TypeScript types for params and searchParams
- SEO metadata (generateMetadata)
- Responsive design
- Accessible

Follow existing project structure.
```

### 2.2 Dynamic route với generateStaticParams

```text
@codebase

Create a dynamic route: /[path]/[id]

Requirements:
- page.tsx with typed params: { params: Promise<{ id: string }> }
- generateStaticParams() for pre-rendering known IDs
- generateMetadata() for dynamic SEO
- Data fetching in Server Component
- Loading state (loading.tsx)
- Not-found handling (notFound())
- Error handling (error.tsx)
```

### 2.3 Parallel routes / Intercepting routes

```text
@codebase

Create [parallel / intercepting] routes for [mô tả: modal, sidebar, dashboard tabs].

Parallel routes:
- @[slot] directories
- default.tsx for unmatched slots
- layout.tsx to compose slots

Intercepting routes:
- (.) for same level
- (..) for one level up
- (...) for root level
- Use for modal patterns (show modal on click, full page on direct URL)

Requirements:
- Typed slot props in layout
- Fallback for unmatched routes
- Loading states per slot
```

### 2.4 Route groups & Layouts

```text
@codebase

Organize routes with route groups for [mô tả: auth layout, dashboard layout, marketing layout].

Structure:
- (auth)/ – login, register, forgot-password (no sidebar)
- (dashboard)/ – main app with sidebar and header
- (marketing)/ – landing pages with different layout

Requirements:
- Shared layout per group
- No URL impact from group names
- Template.tsx if needed (re-mount on navigation)
- Proper navigation between groups
```

---

## 3. Server Components & Server Actions

### 3.1 Server Component với data fetching

```text
@codebase

Create a Server Component for [mô tả page/section].

Requirements:
- async function component
- Fetch data directly (no useEffect)
- Use React cache() if data is shared across components
- Streaming with Suspense for slow data
- Pass serializable props to Client Components
- Error handling (try/catch or error.tsx)
- TypeScript types for fetched data
```

### 3.2 Server Action cho form

```text
@codebase

Create a Server Action for [mô tả: create/update/delete entity].

Requirements:
- 'use server' directive
- Zod validation for input
- Return typed result (success/error, not throw)
- Revalidate affected paths (revalidatePath/revalidateTag)
- Redirect after success (if applicable)
- Error messages for client display
- CSRF protection (built-in with Server Actions)

Integration:
- Form with action={serverAction}
- useActionState for loading/error states
- useOptimistic for optimistic UI
- Progressive enhancement (works without JS)
```

### 3.3 Client Component extraction

```text
@file [filepath]

This Server Component needs interactivity. Extract the minimal Client Component:

Requirements:
- Keep as much as possible in Server Component
- Create separate 'use client' component for interactive parts
- Pass server data as props (serializable only)
- Event handlers in Client Component
- useState/useEffect only in Client Component
- Minimize Client Component bundle size
```

### 3.4 Server Component + TanStack Query

```text
@codebase

Set up TanStack Query with Server Components for [feature]:

Requirements:
- Prefetch data in Server Component
- Hydrate on client with HydrationBoundary
- Client Component uses useQuery (instant, no loading flash)
- Mutations with useMutation + revalidation
- Query key factory
- Type-safe query options
```

---

## 4. Data Fetching & Caching

### 4.1 Data fetching strategy

```text
@codebase

Implement data fetching for [feature/page]:

Strategy decision:
- Static (build time): generateStaticParams + fetch with cache
- ISR (revalidate): fetch with { next: { revalidate: N } }
- Dynamic (every request): fetch with { cache: 'no-store' }
- Client-side: TanStack Query / SWR in Client Component

Requirements:
- Type-safe fetch functions
- Error handling
- Loading states
- Caching with proper revalidation
- Request deduplication (React cache)
```

### 4.2 API integration layer

```text
@codebase

Create an API integration layer for [external API / backend].

Requirements:
- Typed fetch functions for each endpoint
- Base URL from environment variable
- Auth token injection
- Error handling with typed errors
- Request/response logging (dev mode)
- Cache configuration per endpoint
- Revalidation tags for granular cache control

File: lib/api/[entity].ts
```

### 4.3 Caching strategy

```text
@codebase

Implement caching strategy for [feature]:

Requirements:
- Use fetch() cache options appropriately
- Tag-based revalidation (revalidateTag)
- Path-based revalidation (revalidatePath)
- On-demand revalidation from Server Actions
- unstable_cache for non-fetch data (database queries)
- Proper cache headers for CDN
- Document which data is cached and for how long
```

---

## 5. API Routes

### 5.1 Route Handler (app/api)

```text
@codebase

Create API route handlers for [resource]:

File: app/api/[resource]/route.ts

Requirements:
- Export GET, POST, PUT, DELETE functions
- TypeScript types for request/response
- Input validation (zod)
- Proper HTTP status codes and headers
- Error handling with NextResponse.json
- Authentication check
- Rate limiting (if applicable)
- CORS headers (if needed)
```

### 5.2 Dynamic API route

```text
@codebase

Create dynamic API route: app/api/[resource]/[id]/route.ts

Requirements:
- Typed params: { params: Promise<{ id: string }> }
- GET: fetch single resource
- PUT/PATCH: update resource
- DELETE: delete resource
- 404 handling
- Authorization (owner/admin check)
- Input validation
```

### 5.3 Webhook handler

```text
@codebase

Create webhook handler for [service: Stripe, GitHub, Clerk, etc.]:

File: app/api/webhooks/[service]/route.ts

Requirements:
- Signature verification (HMAC / secret)
- Event type routing (switch on event.type)
- Idempotency handling
- Error handling (return 200 even on processing error to avoid retries)
- Logging with event ID
- TypeScript types for events
- Background processing for heavy tasks
```

---

## 6. Authentication & Middleware

### 6.1 Auth setup (NextAuth / Clerk / Lucia)

```text
@codebase

Set up authentication with [NextAuth.js v5 / Clerk / Lucia].

Requirements:
- Auth configuration
- Login/register pages
- Protected routes (middleware)
- Session access in Server Components
- Session access in Client Components
- API route protection
- Role-based access control (if needed)
- TypeScript types for session/user

Follow existing auth patterns or set up from scratch.
```

### 6.2 Middleware

```text
@codebase

Create Next.js middleware for [mô tả: auth protection, redirects, i18n, A/B testing].

File: middleware.ts (root level)

Requirements:
- matcher config for targeted routes
- Minimal logic (middleware runs on every matched request)
- Edge runtime compatible (no Node.js APIs)
- Proper redirects/rewrites
- Headers manipulation if needed
- Auth token check
- TypeScript
```

### 6.3 Protected route pattern

```text
@codebase

Create a protected route pattern for [dashboard / admin / user area]:

Requirements:
- Middleware check for auth token
- Redirect to login if not authenticated
- Layout with auth context
- Role-based protection (admin routes)
- Loading state while checking auth
- Server-side auth check in layouts
```

---

## 7. Performance & SEO

### 7.1 SEO metadata

```text
@codebase

Add comprehensive SEO to [pages/routes]:

Requirements:
- generateMetadata() for dynamic metadata
- Title, description, keywords
- Open Graph tags (og:title, og:description, og:image)
- Twitter Card tags
- Canonical URL
- JSON-LD structured data (schema.org)
- Sitemap (app/sitemap.ts)
- robots.txt (app/robots.ts)
```

### 7.2 Image optimization

```text
@codebase

Optimize images for [page/component]:

Requirements:
- next/image for all images
- Proper width/height/sizes attributes
- Priority for above-the-fold images
- Placeholder="blur" with blurDataURL
- Responsive srcset with sizes prop
- WebP/AVIF format (automatic with next/image)
- Lazy loading for below-the-fold
```

### 7.3 Core Web Vitals optimization

```text
@codebase

Optimize Core Web Vitals for [page]:

Focus:
- LCP: optimize largest content element (images, text blocks)
- FID/INP: minimize JavaScript execution, defer non-critical
- CLS: set dimensions for all media, avoid layout shifts

Actions:
- Use next/font (avoid FOIT/FOUT)
- Use next/image with proper sizes
- Stream with Suspense
- Lazy load below-the-fold components
- Minimize Client Component JavaScript
- Add loading.tsx for route transitions
```

---

## 8. Debug Prompts

### 8.1 Hydration error

```text
@terminal
@file [filepath]

Next.js hydration error:
[paste error: "Hydration failed", "Text content mismatch", etc.]

Please:
- Identify server/client rendering difference
- Common causes: Date, Math.random, window/document in render
- Fix: move to useEffect, or use suppressHydrationWarning (last resort)
- Check dynamic imports with { ssr: false } for client-only components
```

### 8.2 Build error

```text
@terminal
@codebase

Next.js build error:
[paste: next build error output]

Please:
- Identify the build issue
- Fix: missing 'use client', invalid Server Component import, type error
- Check: dynamic route params, generateStaticParams, metadata
- Verify: server-only code not imported in client
```

### 8.3 Server Component error

```text
@terminal
@file [filepath]

Server Component error:
[paste error]

Common issues:
- Using hooks (useState, useEffect) in Server Component
- Importing client-only library in Server Component
- Passing non-serializable props to Client Component
- Event handlers in Server Component

Please fix and explain the server/client boundary.
```

### 8.4 Caching / Stale data

```text
@codebase

Data is stale / not updating after mutation:
[mô tả: form submit but old data shown, cache not invalidated]

Please:
- Check fetch cache configuration
- Add revalidatePath/revalidateTag in Server Action
- Check if using 'no-store' where needed
- Verify ISR revalidation timing
- Check TanStack Query cache invalidation
```

### 8.5 Middleware / Redirect loop

```text
@terminal
@file middleware.ts

Redirect loop or middleware issue:
[paste error or describe: infinite redirect, wrong redirect, middleware not matching]

Please:
- Check matcher configuration
- Check redirect conditions (avoid circular redirects)
- Verify auth check logic
- Debug with console.log in middleware
- Check NextResponse.next() vs NextResponse.redirect()
```

---

## 9. Testing Prompts

### 9.1 Component tests

```text
@codebase

Write tests for Next.js component @file [filepath].

Requirements:
- React Testing Library + Vitest/Jest
- Mock next/navigation (useRouter, usePathname, useSearchParams)
- Mock next/image
- Test Server Component rendering (if applicable)
- Test Client Component interactivity
- Test loading/error states
- Mock fetch for data
```

### 9.2 API route tests

```text
@codebase

Write tests for API route @file [route handler filepath].

Requirements:
- Test each HTTP method (GET, POST, PUT, DELETE)
- Test request validation
- Test auth protection
- Test error responses
- Mock database/external services
- Typed request/response assertions
```

### 9.3 E2E tests (Playwright)

```text
@codebase

Write Playwright E2E tests for [flow/page].

Requirements:
- Full user flow testing
- Navigation between pages
- Form submission
- Auth flow (login → protected page)
- API mocking for deterministic tests
- Mobile/desktop viewports
- Accessibility checks
- Screenshot comparison (if applicable)
```

---

## 10. Best Practices & Anti-patterns

### ✅ DO

- Server Components by default, `'use client'` only when needed
- Fetch data in Server Components (not useEffect)
- Use Server Actions for mutations
- Use `loading.tsx` and `error.tsx` per route segment
- Use `generateMetadata()` for SEO
- Use `next/image`, `next/link`, `next/font`
- Colocate components with their route segment
- Stream with Suspense for slow data
- Use route groups `(group)` for layout organization
- Type params and searchParams as Promise (Next.js 15)

### ❌ DON'T

- Không dùng `'use client'` ở layout.tsx (trừ khi thực sự cần)
- Không fetch data trong Client Components nếu có thể fetch ở server
- Không dùng `useEffect` cho data fetching (dùng Server Components hoặc React Query)
- Không import server-only code trong Client Components
- Không dùng `<img>` tag – dùng `next/image`
- Không dùng `<a>` tag cho internal links – dùng `next/link`
- Không dùng `getServerSideProps` / `getStaticProps` (Pages Router API) trong App Router
- Không put secrets trong `NEXT_PUBLIC_` env vars
- Không dùng `router.push` cho simple navigation – dùng `<Link>`
- Không skip `loading.tsx` – UX rất xấu khi không có loading state

---

> 📌 Quay lại [JavaScript](./README.md) | [Handbook chính](../cursor-ide-handbook.md) | Xem thêm: [React](./react.md) | [NestJS](./nestjs.md)
