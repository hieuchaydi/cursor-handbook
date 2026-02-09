# 📘 CURSOR PROMPTS – React

**Rules • Prompts • Debug • Best Practices cho React trong Cursor IDE**

> File này thuộc thư mục [javascript/](./README.md) – hệ sinh thái JS/TS.

---

## Mục lục

- [1. Rules template cho React](#1-rules-template-cho-react)
- [2. Component Prompts](#2-component-prompts)
- [3. Hooks & State Management](#3-hooks--state-management)
- [4. Data Fetching & Server State](#4-data-fetching--server-state)
- [5. Forms & Validation](#5-forms--validation)
- [6. Performance Optimization](#6-performance-optimization)
- [7. Debug Prompts](#7-debug-prompts)
- [8. Testing Prompts](#8-testing-prompts)
- [9. Best Practices & Anti-patterns](#9-best-practices--anti-patterns)

---

## 1. Rules template cho React

### 1.1 File `.cursor/rules/react.mdc`

```
---
description: Rules for React component files
globs: *.tsx, *.jsx
---

- Use functional components only (no class components)
- Use TypeScript with strict types for all props and state
- Define props with interface (not type alias for component props)
- Use React.FC sparingly – prefer explicit return type or inference
- Keep components small (< 150 lines) and focused
- Extract logic into custom hooks
- Use named exports (not default exports)
- One component per file (+ small sub-components if tightly coupled)
- Props interface name: [ComponentName]Props
- Use React 19 features when available (use, Actions, etc.)
- Use Tailwind CSS / CSS Modules following project convention
- Handle loading, error, and empty states
- Memoize expensive computations (useMemo) and callbacks (useCallback) only when needed
- Use Suspense + Error Boundaries for async content
- Accessible by default: semantic HTML, aria labels, keyboard navigation
```

### 1.2 Full `.cursorrules` cho React project

```txt
You are an expert React developer with deep knowledge of React 19,
TypeScript, modern hooks patterns, and component architecture.

# React Rules
- Functional components only. No class components.
- TypeScript strict mode. No 'any'.
- Props defined as interface [ComponentName]Props.
- Named exports for all components.
- Keep components < 150 lines. Extract hooks for logic.
- Handle all UI states: loading, error, empty, success.
- Use composition over configuration (children, render props, slots).

# Hooks Rules
- Custom hooks prefix: use[Name].
- Never call hooks conditionally or in loops.
- Extract complex state logic into custom hooks.
- Use useCallback/useMemo only when needed (measure first).
- Prefer useReducer for complex state transitions.

# Styling
- Follow project convention: [Tailwind CSS / CSS Modules / styled-components].
- Use responsive design (mobile-first).
- Follow design system tokens if available.

# State Management
- Local state: useState / useReducer.
- Server state: TanStack Query (React Query) / SWR.
- Global state: [Zustand / Jotai / Redux Toolkit] (if project uses one).
- URL state: useSearchParams.
- Form state: React Hook Form / useActionState.

# Performance
- Lazy load routes and heavy components (React.lazy + Suspense).
- Virtualize long lists (TanStack Virtual / react-window).
- Debounce/throttle expensive handlers.
- Avoid re-renders: proper key usage, state colocation.

# Accessibility
- Semantic HTML elements.
- ARIA labels and roles where needed.
- Keyboard navigation support.
- Color contrast compliance.

# Output Format
- Always start with: "Files changed/created:"
- List each file path clearly.
- Show component hierarchy if creating multiple components.
```

---

## 2. Component Prompts

### 2.1 Tạo component mới

```text
@codebase

Create a React component for [mô tả UI/chức năng].

Requirements:
- TypeScript with strict props interface
- Named export
- Handle states: loading, error, empty, success
- Responsive design (mobile-first)
- Accessible (semantic HTML, aria labels, keyboard)
- Styled with [Tailwind / CSS Modules / styled-components]
- Props with sensible defaults
- Storybook story (if project uses Storybook)

Follow existing component patterns in the project.
```

### 2.2 Tạo form component

```text
@codebase

Create a form component for [mô tả: user registration, product creation, settings...].

Requirements:
- Use [React Hook Form / native form + useActionState]
- Validation with [zod / yup] schema
- Typed form values
- Field-level error messages
- Submit handling (async, loading state, error state)
- Disabled state during submission
- Accessible form labels and error announcements
- Server-side validation error handling
- Reset form on success (if applicable)

Follow existing form patterns.
```

### 2.3 Tạo layout component

```text
@codebase

Create a layout component for [mô tả: dashboard, sidebar + main, header + footer...].

Requirements:
- Responsive design (mobile sidebar collapse, etc.)
- Composition with children / slots
- Navigation integration
- Active route highlighting
- Breadcrumbs if applicable
- Theme support (dark/light) if project uses it
- Semantic HTML (header, main, nav, aside, footer)
```

### 2.4 Tạo data table component

```text
@codebase

Create a data table component for [entity/data type].

Requirements:
- Use [TanStack Table / custom implementation]
- TypeScript generic for row data type
- Features: sorting, filtering, pagination
- Column definitions with type safety
- Row selection (single/multi) if needed
- Responsive (horizontal scroll on mobile)
- Loading skeleton
- Empty state
- Accessible table markup
```

### 2.5 Tạo modal / dialog

```text
@codebase

Create a modal/dialog component for [mô tả].

Requirements:
- Portal rendering (outside main DOM tree)
- Backdrop click to close
- Escape key to close
- Focus trap inside modal
- Proper ARIA: role="dialog", aria-modal, aria-labelledby
- Animation (enter/exit)
- Prevent body scroll when open
- Composable: Header, Body, Footer slots
- Controlled (open/onClose props)
```

### 2.6 Refactor component

```text
@file [filepath]

Refactor this React component:
- Extract complex logic into custom hooks
- Split into smaller sub-components if > 150 lines
- Add TypeScript types for all props and state
- Handle loading/error/empty states
- Add proper memoization only where needed
- Improve accessibility
- Remove unnecessary re-renders (state colocation)

Keep exact same visual behavior.
```

### 2.7 Convert class component to functional

```text
@file [filepath]

Convert this class component to a modern functional component:
- Replace lifecycle methods with hooks (useEffect, useMemo, etc.)
- Replace this.state with useState/useReducer
- Replace this.props with destructured props
- Replace class methods with functions/callbacks
- Replace HOCs with hooks if applicable
- Add TypeScript types
- Keep exact same behavior
```

---

## 3. Hooks & State Management

### 3.1 Tạo custom hook

```text
@codebase

Create a custom hook: use[Name] for [mô tả chức năng].

Requirements:
- TypeScript return type (tuple or object)
- Proper cleanup in useEffect (return cleanup function)
- Error handling
- Loading state if async
- Memoized callbacks if exposed
- JSDoc comment with usage example
- Unit test

Follow existing hook patterns.
```

### 3.2 Tạo useForm hook

```text
@codebase

Create a useForm custom hook for [form type].

Requirements:
- Generic type for form values
- Field registration (name, value, onChange, error)
- Validation (sync + async)
- Dirty/touched tracking
- Submit handler with loading state
- Reset functionality
- Field-level and form-level errors
- TypeScript type-safe field names
```

### 3.3 Zustand store

```text
@codebase

Create a Zustand store for [mô tả state].

Requirements:
- TypeScript interface for state and actions
- Slices pattern if complex
- Derived state with selectors
- Async actions with error handling
- Persist middleware (if needed)
- DevTools middleware (dev mode)
- Unit test for actions

Follow existing store patterns.
```

### 3.4 Redux Toolkit slice

```text
@codebase

Create a Redux Toolkit slice for [mô tả feature].

Requirements:
- createSlice with typed state
- Reducers for sync actions
- createAsyncThunk for async actions
- Selectors (createSelector for memoized)
- TypeScript: RootState, AppDispatch typed
- Loading/error/data pattern
- Optimistic updates if applicable
- Unit test for reducers

Register in store.
```

### 3.5 Context + useReducer pattern

```text
@codebase

Create a Context + useReducer pattern for [mô tả: theme, auth, cart, notifications].

Requirements:
- Context with Provider component
- useReducer with typed actions (discriminated union)
- Custom hook (use[Feature]) to consume context
- Error if used outside Provider
- Memoized context value (prevent unnecessary re-renders)
- TypeScript types for state, action, dispatch
```

---

## 4. Data Fetching & Server State

### 4.1 TanStack Query (React Query) setup

```text
@codebase

Create data fetching hooks with TanStack Query for [entity/API].

Requirements:
- useQuery for GET operations
- useMutation for POST/PUT/DELETE
- Query key factory pattern
- TypeScript generic types for data
- Loading/error/data states
- Cache invalidation strategy
- Optimistic updates for mutations
- Infinite query for pagination (if applicable)
- Prefetching for anticipated navigations
```

### 4.2 SWR hooks

```text
@codebase

Create SWR hooks for [entity/API].

Requirements:
- useSWR for data fetching
- useSWRMutation for mutations
- Typed fetcher function
- Error handling
- Revalidation strategy
- Optimistic UI updates
- Global configuration
```

### 4.3 Server Actions (React 19)

```text
@codebase

Create Server Actions for [feature].

Requirements:
- 'use server' directive
- Typed input/output
- Validation with zod
- Error handling (return Result, not throw)
- useActionState for form integration
- useOptimistic for optimistic UI
- Revalidation after mutation
- Progressive enhancement (works without JS)
```

---

## 5. Forms & Validation

### 5.1 React Hook Form + Zod

```text
@codebase

Create a form with React Hook Form + Zod for [mô tả].

Requirements:
- Zod schema with custom error messages
- useForm with zodResolver
- TypeScript: infer types from schema
- Controlled components where needed
- Dynamic fields (useFieldArray) if needed
- Watch for dependent fields
- Submit handling with loading/error states
- Server-side error integration
```

### 5.2 Multi-step form / Wizard

```text
@codebase

Create a multi-step form wizard for [mô tả flow].

Requirements:
- Step navigation (next, back, skip if allowed)
- Per-step validation
- Persist state across steps
- Progress indicator
- Summary/review step before submit
- Type-safe step data
- Accessible (announce step changes)
```

---

## 6. Performance Optimization

### 6.1 Diagnose re-render issues

```text
@file [filepath]

This component re-renders too often. Diagnose and fix:

Please:
- Identify causes of unnecessary re-renders
- Apply: state colocation, memo, useMemo, useCallback where needed
- Split component if too much state
- Use React DevTools Profiler findings if provided
- Don't over-optimize – only fix actual performance issues
```

### 6.2 Lazy loading setup

```text
@codebase

Add lazy loading for [routes / heavy components / images].

Requirements:
- React.lazy + Suspense for code splitting
- Route-based splitting
- Loading fallback component
- Error boundary for chunk load failures
- Prefetch on hover/focus (if applicable)
- Bundle analyzer to verify split
```

### 6.3 Virtualized list

```text
@codebase

Create a virtualized list for [mô tả: large data set, infinite scroll, chat messages].

Requirements:
- Use [TanStack Virtual / react-window / react-virtuoso]
- Variable height items (if needed)
- Infinite scroll with data fetching
- Scroll to item functionality
- Keyboard navigation
- Proper cleanup on unmount
```

---

## 7. Debug Prompts

### 7.1 Component not rendering / wrong output

```text
@file [filepath]

Component [name] is not rendering correctly:
[mô tả: blank, wrong data, stale data, flickering]

Please:
- Check props/state flow
- Check conditional rendering logic
- Check key prop usage in lists
- Verify data fetching and state updates
- Check for stale closures in useEffect
```

### 7.2 useEffect infinite loop

```text
@file [filepath]

useEffect is running in an infinite loop:
[paste console output or describe]

Please:
- Check dependency array
- Fix object/array references in deps (useMemo/useCallback)
- Ensure state updates in effect don't trigger same effect
- Use useRef for values that shouldn't trigger re-render
```

### 7.3 State not updating

```text
@file [filepath]

State is not updating as expected:
[mô tả: stale closure, batching issue, async state]

Please:
- Check for stale closures (use functional setState)
- Check async state updates
- Check if state is being mutated (instead of replaced)
- Verify useEffect dependencies
```

### 7.4 Hydration mismatch (SSR)

```text
@terminal
@file [filepath]

Hydration error:
[paste error: "Text content did not match" / "Hydration failed"]

Please:
- Identify server/client rendering difference
- Fix: use useEffect for client-only code
- Use suppressHydrationWarning only as last resort
- Check for: Date, random values, browser APIs in render
```

---

## 8. Testing Prompts

### 8.1 Component tests (React Testing Library)

```text
@codebase

Write tests for @file [filepath] using React Testing Library.

Requirements:
- render + screen queries (getByRole, getByText, etc.)
- User interaction with userEvent
- Test all UI states (loading, error, empty, data)
- Test form submission
- Mock API calls (MSW or vi.mock)
- Test accessibility (no a11y violations)
- Async assertions (waitFor, findBy)
- Naming: describe("[Component]") > it("should [behavior]")
```

### 8.2 Hook tests

```text
@codebase

Write tests for custom hook: use[Name].

Requirements:
- Use renderHook from @testing-library/react
- Test initial state
- Test state transitions (act + result.current)
- Test async operations
- Test cleanup on unmount
- Test error cases
- Wrapper for providers if needed
```

### 8.3 Storybook stories

```text
@codebase

Create Storybook stories for @file [component filepath].

Requirements:
- CSF3 format (meta + StoryObj)
- Stories for each variant/state
- Args with controls
- Play functions for interaction tests
- Decorators for providers/context
- Documentation (description, argTypes)
- Responsive viewport stories
```

---

## 9. Best Practices & Anti-patterns

### ✅ DO

- Dùng functional components + hooks
- Dùng TypeScript strict cho mọi props/state
- Dùng composition (children, render props) thay vì prop drilling
- Dùng React.lazy + Suspense cho code splitting
- Dùng Error Boundaries cho error handling
- Dùng `key` prop đúng (stable, unique ID – **không phải** index)
- Dùng semantic HTML (button, a, nav, main, etc.)
- Colocate state gần nơi sử dụng nhất
- Extract custom hooks cho reusable logic
- Test behavior, không test implementation details

### ❌ DON'T

- Không dùng class components (trừ Error Boundaries nếu cần)
- Không dùng `any` cho props/state
- Không dùng `useEffect` cho derived state – dùng `useMemo` hoặc compute in render
- Không dùng `useEffect` cho event handlers
- Không dùng index làm key trong list có thể thay đổi
- Không mutate state trực tiếp (dùng spread hoặc immer)
- Không dùng `useMemo`/`useCallback` cho mọi thứ (premature optimization)
- Không dùng `dangerouslySetInnerHTML` với user input
- Không fetch data trong useEffect nếu có React Query/SWR
- Không đặt business logic trong components – đặt trong hooks/services

---

> 📌 Quay lại [JavaScript](./README.md) | [Handbook chính](../cursor-ide-handbook.md) | Xem thêm: [Next.js](./nextjs.md) | [Vue](./vue.md)
