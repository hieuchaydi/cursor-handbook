# 📘 CURSOR PROMPTS – Vue.js

**Rules • Prompts • Debug • Best Practices cho Vue.js 3 + Nuxt trong Cursor IDE**

> File này thuộc thư mục [javascript/](./README.md) – hệ sinh thái JS/TS.

---

## Mục lục

- [1. Rules template cho Vue](#1-rules-template-cho-vue)
- [2. Component Prompts](#2-component-prompts)
- [3. Composables & State Management](#3-composables--state-management)
- [4. Nuxt 3 Prompts](#4-nuxt-3-prompts)
- [5. Forms & Validation](#5-forms--validation)
- [6. Debug Prompts](#6-debug-prompts)
- [7. Testing Prompts](#7-testing-prompts)
- [8. Best Practices & Anti-patterns](#8-best-practices--anti-patterns)

---

## 1. Rules template cho Vue

### 1.1 File `.cursor/rules/vue.mdc`

```
---
description: Rules for Vue.js component files
globs: *.vue, composables/*.ts, stores/*.ts
---

- Use Vue 3 Composition API with <script setup lang="ts">
- Use TypeScript strict mode, no 'any'
- Use defineProps with TypeScript generics: defineProps<{...}>()
- Use defineEmits with TypeScript: defineEmits<{...}>()
- Use ref(), reactive(), computed(), watch(), watchEffect()
- Extract reusable logic into composables (use[Name])
- Use Pinia for global state management
- Use auto-imports (if Nuxt or unplugin-auto-import configured)
- Keep components small (< 200 lines including template)
- One component per .vue file
- Use <template>, <script setup>, <style scoped> order
- Handle loading, error, and empty states
- Use semantic HTML and accessibility best practices
```

### 1.2 Full `.cursorrules` cho Vue project

```txt
You are an expert Vue.js developer with deep knowledge of Vue 3,
Composition API, TypeScript, Pinia, Vue Router, and Nuxt 3.

# Vue 3 Rules
- Composition API only (<script setup lang="ts">).
- No Options API for new code.
- TypeScript strict mode in all files.
- No 'any' type.

# Component Structure
- <script setup lang="ts"> (first or after template, follow project)
- <template> with semantic HTML
- <style scoped> (or CSS Modules)
- Keep components < 200 lines
- One component per file

# Props & Emits
- defineProps<{...}>() with TypeScript interface
- defineEmits<{...}>() with TypeScript
- withDefaults() for default values
- Props are readonly – never mutate

# Reactivity
- ref() for primitives, reactive() for objects
- computed() for derived state
- watch() / watchEffect() for side effects
- toRef() / toRefs() for destructuring reactive objects
- Use shallowRef/shallowReactive for large objects if needed

# Composables
- Prefix: use[Name] (useAuth, useCounter, useFetch)
- Return reactive refs and methods
- Cleanup side effects (onUnmounted / watchEffect auto-cleanup)
- Type return value explicitly

# State Management
- Local state: ref/reactive in component
- Shared state: Pinia stores
- Server state: useFetch (Nuxt) / TanStack Query / VueQuery
- URL state: useRoute().query

# Output Format
- Always start with: "Files changed/created:"
- List each file path clearly.
- Show template and script separately if large.
```

---

## 2. Component Prompts

### 2.1 Tạo component mới

```text
@codebase

Create a Vue 3 component for [mô tả UI/chức năng].

Requirements:
- <script setup lang="ts">
- TypeScript props with defineProps<{...}>()
- Emits with defineEmits<{...}>()
- Handle states: loading, error, empty, success
- Responsive design (mobile-first)
- Accessible (semantic HTML, aria labels)
- Styled with [Tailwind / SCSS / CSS Modules] following project convention
- Scoped styles
- Proper v-model support if it's a form control

Follow existing component patterns.
```

### 2.2 Tạo form component

```text
@codebase

Create a form component for [mô tả: login, registration, product creation].

Requirements:
- <script setup lang="ts">
- v-model binding for form fields
- Validation with [VeeValidate + zod / FormKit / custom]
- Field-level error messages
- Form-level validation on submit
- Loading state during submission
- Server-side error handling
- Disable form during submit
- Accessible form labels and error descriptions
- Reset on success
```

### 2.3 Tạo layout component

```text
@codebase

Create a layout component for [mô tả: sidebar layout, dashboard, admin panel].

Requirements:
- <slot> for main content
- Named slots for header, sidebar, footer if needed
- Responsive (mobile menu toggle)
- Navigation with <RouterLink>
- Active route highlighting
- Transition animations
- Semantic HTML (header, nav, main, aside, footer)
```

### 2.4 Tạo renderless / headless component

```text
@codebase

Create a renderless component for [mô tả: dropdown, modal logic, infinite scroll, drag-drop].

Requirements:
- No template – expose via scoped slot or composable
- Full logic encapsulated
- TypeScript types for slot props
- Accessible keyboard handling
- Example usage with custom template
- Can be used as composable alternative
```

### 2.5 Component với v-model

```text
@codebase

Create a component with v-model support for [mô tả: custom input, select, toggle, slider].

Requirements:
- defineModel() (Vue 3.4+) or modelValue prop + update:modelValue emit
- Multiple v-model bindings if needed (v-model:title, v-model:content)
- Validation integration
- Disabled/readonly states
- Accessible
- Proper type definitions
```

### 2.6 Refactor Options API to Composition API

```text
@file [filepath]

Refactor this Vue component from Options API to Composition API:
- Convert to <script setup lang="ts">
- Replace data() with ref()/reactive()
- Replace computed: {} with computed()
- Replace methods: {} with functions
- Replace watch: {} with watch()/watchEffect()
- Replace mounted/created with onMounted/immediate watch
- Replace mixins with composables
- Add TypeScript types for all props/emits/refs
- Convert this.$emit to defineEmits
- Convert this.$refs to template refs

Keep exact same behavior and template.
```

---

## 3. Composables & State Management

### 3.1 Tạo composable

```text
@codebase

Create a composable: use[Name] for [mô tả chức năng].

Requirements:
- Function that returns reactive state + methods
- TypeScript return type
- ref/reactive for state
- Cleanup on component unmount (onUnmounted, or auto-cleanup with watchEffect)
- Error handling
- Loading state if async
- Reusable across components
- JSDoc comment with usage example

File: composables/use[Name].ts
```

### 3.2 Tạo useFetch composable (custom)

```text
@codebase

Create a useFetch composable for type-safe API calls:

Requirements:
- Generic type for response: useFetch<T>(url)
- Return: { data, error, loading, refetch }
- All returns are Ref<T>
- AbortController for cancellation
- Retry logic
- Caching (optional)
- Error typing
- Auto-fetch on param change (watch URL)
- SSR-compatible (if Nuxt)
```

### 3.3 Pinia Store

```text
@codebase

Create a Pinia store for [feature]:

Requirements:
- Setup store syntax (defineStore with setup function)
- State as ref/reactive
- Getters as computed
- Actions as functions
- Async actions with loading/error state
- TypeScript types for state
- Persist plugin (if needed)
- Reset action
- Unit test

File: stores/use[Feature]Store.ts
```

### 3.4 Pinia Store (Option syntax)

```text
@codebase

Create a Pinia store for [feature]:

Requirements:
- Option syntax: state, getters, actions
- TypeScript interface for state
- Async actions with error handling
- $patch for batch updates
- Plugin integration (persist, devtools)
- Subscribe to state changes ($subscribe) if needed
```

### 3.5 Provide/Inject pattern

```text
@codebase

Create a provide/inject pattern for [mô tả: theme, config, auth context]:

Requirements:
- InjectionKey with TypeScript type
- Provider component or composable
- Consumer composable (useXxx)
- Error if consumed outside provider
- Reactive provided values
- Type-safe injection
```

---

## 4. Nuxt 3 Prompts

### 4.1 Tạo Nuxt page

```text
@codebase

Create a Nuxt 3 page for [mô tả]:

Route: /[path]

Requirements:
- File-based routing (pages/[path].vue)
- definePageMeta for layout, middleware, etc.
- useFetch / useAsyncData for server-side data fetching
- SEO with useHead / useSeoMeta
- Loading state handling
- Error handling (NuxtErrorBoundary or showError)
- TypeScript types for route params
```

### 4.2 Nuxt API route (server/)

```text
@codebase

Create Nuxt server API routes for [resource]:

Files: server/api/[resource]/
- index.get.ts – List
- index.post.ts – Create
- [id].get.ts – Get by ID
- [id].put.ts – Update
- [id].delete.ts – Delete

Requirements:
- defineEventHandler
- Input validation (zod + readValidatedBody/getValidatedQuery)
- Proper HTTP status codes (setResponseStatus)
- Error handling (createError)
- TypeScript types
- Database integration
```

### 4.3 Nuxt middleware

```text
@codebase

Create Nuxt middleware for [mô tả: auth, locale, redirect]:

Requirements:
- defineNuxtRouteMiddleware
- Named middleware (middleware/[name].ts) or inline
- Global middleware (middleware/[name].global.ts)
- navigateTo for redirects
- Access to route and cookies
- Server-side compatible
```

### 4.4 Nuxt composable (auto-imported)

```text
@codebase

Create a Nuxt composable: use[Name]

Requirements:
- File in composables/ (auto-imported)
- SSR-compatible (no browser-only APIs without check)
- Use useState for SSR-safe state
- Use useFetch/useAsyncData for data
- Use useCookie for cookie access
- Proper hydration handling
- TypeScript types
```

### 4.5 Nuxt plugin

```text
@codebase

Create a Nuxt plugin for [mô tả: analytics, auth, i18n, toast]:

Requirements:
- defineNuxtPlugin
- Client-only or universal (specify)
- Provide helper via nuxtApp.provide
- Type augmentation (declare module '#app')
- Hook into app lifecycle if needed
- Configuration from runtimeConfig
```

### 4.6 Nuxt module (custom)

```text
@codebase

Create a Nuxt module for [mô tả]:

Requirements:
- defineNuxtModule with meta and setup
- Module options with defaults
- Add server handlers, plugins, composables
- Type declarations
- Auto-import configuration
- README with usage instructions
```

### 4.7 Nuxt SEO

```text
@codebase

Add comprehensive SEO to [pages]:

Requirements:
- useSeoMeta for standard meta tags
- useHead for advanced head management
- Open Graph + Twitter Card tags
- JSON-LD structured data (useSchemaOrg)
- Dynamic metadata based on content
- Canonical URLs
- Sitemap (nuxt-simple-sitemap)
- Robots.txt configuration
```

---

## 5. Forms & Validation

### 5.1 VeeValidate + Zod

```text
@codebase

Create a form with VeeValidate + Zod for [mô tả]:

Requirements:
- useForm() with zod schema (toTypedSchema)
- useField() for individual fields
- Typed form values
- Field-level error messages
- handleSubmit with async handler
- Loading/error states
- Server error integration
- Accessible form structure
```

### 5.2 FormKit form

```text
@codebase

Create a FormKit form for [mô tả]:

Requirements:
- FormKit schema or template approach
- Validation rules (built-in + custom)
- Multi-step form (if applicable)
- Custom input components
- Styled with project theme
- TypeScript types for form data
- Submit handling
```

---

## 6. Debug Prompts

### 6.1 Reactivity not working

```text
@file [filepath]

Vue reactivity issue:
[mô tả: value not updating, computed not recalculating, watch not triggering]

Please:
- Check if using ref() vs reactive() correctly
- Check if destructuring lost reactivity (need toRefs)
- Check if replacing entire reactive object (need to assign properties)
- Check computed dependencies
- Check watch source (ref needs .value, reactive doesn't)
- Verify v-model binding
```

### 6.2 Component not re-rendering

```text
@file [filepath]

Component not re-rendering:
[mô tả: prop changed but UI doesn't update]

Please:
- Check if prop is properly reactive
- Check if array/object is mutated instead of replaced
- Check :key prop on list items
- Check v-if/v-show conditions
- Verify parent re-renders pass new reference
```

### 6.3 Nuxt hydration mismatch

```text
@terminal
@file [filepath]

Nuxt hydration mismatch:
[paste error]

Please:
- Identify server/client rendering difference
- Fix: use <ClientOnly>, onMounted for client-only code
- Check: Date, random values, browser APIs in template
- Use useState instead of ref for SSR-safe state
- Check v-if with client-only conditions
```

### 6.4 Pinia state issue

```text
@codebase
@file [store file]

Pinia store issue:
[mô tả: state not updating, state lost on navigation, stale state]

Please:
- Check if using $patch or direct mutation
- Check if subscribing correctly
- Check SSR: use useState or pinia-plugin-persistedstate
- Verify store is properly defined and used
- Check for reactive unwrapping issues
```

### 6.5 Vue Router issue

```text
@codebase

Router issue:
[mô tả: wrong route, guard blocking, params not reactive, navigation failure]

Please:
- Check route configuration order
- Check guard logic and return values
- Use route params with watch or computed (they're reactive)
- Check for navigation failures (router.push returns Promise)
- Verify dynamic route matching
```

---

## 7. Testing Prompts

### 7.1 Component tests (Vitest + Vue Test Utils)

```text
@codebase

Write tests for @file [filepath].

Requirements:
- Vitest + @vue/test-utils
- mount/shallowMount with proper options
- Test rendering in different states (props variations)
- Test user interactions (trigger, setValue)
- Test emits (wrapper.emitted())
- Mock composables/stores
- Test slots
- Test async operations (flushPromises, nextTick)
- Naming: describe("[Component]") > it("should [behavior]")
```

### 7.2 Composable tests

```text
@codebase

Write tests for composable: use[Name].

Requirements:
- Test with withSetup helper or renderHook equivalent
- Test reactive state changes
- Test computed values
- Test methods/actions
- Test cleanup (onUnmounted)
- Test async operations
- Mock dependencies
```

### 7.3 Pinia Store tests

```text
@codebase

Write tests for Pinia store @file [filepath].

Requirements:
- createTestingPinia or setActivePinia(createPinia())
- Test initial state
- Test getters (computed)
- Test actions (sync and async)
- Test state mutations
- Mock API calls
- Test error handling
- Test store interactions (if stores depend on each other)
```

### 7.4 Nuxt tests

```text
@codebase

Write tests for Nuxt [page / composable / server API]:

Requirements:
- @nuxt/test-utils for Nuxt testing
- mountSuspended for async components
- mockNuxtImport for auto-imports
- registerEndpoint for API mocking
- Test SSR behavior
- Test useFetch/useAsyncData
```

### 7.5 E2E tests (Playwright / Cypress)

```text
@codebase

Write E2E tests for [feature/page]:

Requirements:
- Framework: [Playwright / Cypress]
- Test user journey
- Test navigation
- Test form submission and validation
- Test responsive design
- Mock API for deterministic tests
- Accessibility checks
```

---

## 8. Best Practices & Anti-patterns

### ✅ DO

- Dùng `<script setup lang="ts">` cho mọi component
- Dùng `defineProps<{...}>()` với TypeScript generics
- Dùng `defineEmits<{...}>()` với TypeScript
- Dùng `defineModel()` (Vue 3.4+) cho v-model
- Dùng `ref()` cho primitives, `computed()` cho derived state
- Dùng composables (`use[Name]`) cho reusable logic
- Dùng Pinia Setup Stores cho global state
- Dùng `watchEffect()` khi muốn auto-track dependencies
- Dùng `toRefs()` khi destructure reactive object
- Dùng `shallowRef()` cho large objects không cần deep reactivity
- Dùng `<Suspense>` + async setup cho loading states
- Dùng `<Teleport>` cho modals/tooltips
- Key prop đúng trong `v-for` (unique ID, không phải index)

### ❌ DON'T

- Không dùng Options API cho code mới
- Không dùng `any` type
- Không mutate props (Vue sẽ warn, nhưng đừng tìm cách vượt qua)
- Không dùng `v-html` với user input (XSS)
- Không dùng `$refs` trong `<script setup>` – dùng `ref()` template refs
- Không dùng `this` trong `<script setup>` (không có `this`)
- Không destructure props trực tiếp (mất reactivity) – dùng `toRefs(props)` hoặc `computed()`
- Không dùng `watch()` khi `computed()` đủ
- Không dùng `reactive()` cho primitives (dùng `ref()`)
- Không dùng index làm `:key` trong `v-for` có thể thay đổi
- Không quên cleanup side effects (event listeners, intervals, subscriptions)
- Không dùng `created()`/`mounted()` hooks – dùng `onMounted()` trong setup

### Ví dụ: Clean Vue 3 Component

```vue
<script setup lang="ts">
import { ref, computed, onMounted } from 'vue';
import { useUserStore } from '@/stores/useUserStore';
import { useNotification } from '@/composables/useNotification';
import type { User } from '@/types';

interface Props {
  userId: string;
  showAvatar?: boolean;
}

const props = withDefaults(defineProps<Props>(), {
  showAvatar: true,
});

const emit = defineEmits<{
  select: [user: User];
  delete: [userId: string];
}>();

const userStore = useUserStore();
const { notify } = useNotification();

const isLoading = ref(false);
const error = ref<string | null>(null);

const user = computed(() => userStore.getUserById(props.userId));
const fullName = computed(() =>
  user.value ? `${user.value.firstName} ${user.value.lastName}` : '',
);

async function handleDelete() {
  if (!confirm('Are you sure?')) return;

  isLoading.value = true;
  error.value = null;

  try {
    await userStore.deleteUser(props.userId);
    emit('delete', props.userId);
    notify({ type: 'success', message: 'User deleted' });
  } catch (err) {
    error.value = err instanceof Error ? err.message : 'Failed to delete user';
    notify({ type: 'error', message: error.value });
  } finally {
    isLoading.value = false;
  }
}

onMounted(() => {
  if (!user.value) {
    userStore.fetchUser(props.userId);
  }
});
</script>

<template>
  <div class="user-card">
    <template v-if="user">
      <img
        v-if="showAvatar"
        :src="user.avatarUrl"
        :alt="`${fullName}'s avatar`"
        class="user-card__avatar"
      />
      <div class="user-card__info">
        <h3>{{ fullName }}</h3>
        <p>{{ user.email }}</p>
      </div>
      <div class="user-card__actions">
        <button @click="emit('select', user)" :disabled="isLoading">
          Select
        </button>
        <button @click="handleDelete" :disabled="isLoading" class="danger">
          {{ isLoading ? 'Deleting...' : 'Delete' }}
        </button>
      </div>
      <p v-if="error" class="error" role="alert">{{ error }}</p>
    </template>
    <div v-else class="user-card__skeleton" aria-busy="true">
      Loading...
    </div>
  </div>
</template>

<style scoped>
.user-card { /* ... */ }
.user-card__avatar { /* ... */ }
.danger { color: var(--color-danger); }
.error { color: var(--color-danger); font-size: 0.875rem; }
</style>
```

---

> 📌 Quay lại [JavaScript](./README.md) | [Handbook chính](../cursor-ide-handbook.md) | Xem thêm: [React](./react.md) | [Angular](./angular.md)
