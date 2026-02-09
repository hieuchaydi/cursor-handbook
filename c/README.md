# 📘 CURSOR PROMPTS – C Core

**Rules • Prompts • Debug • Best Practices cho ngôn ngữ C trong Cursor IDE**

> Thư mục này là phần mở rộng của [cursor-ide-handbook.md](../cursor-ide-handbook.md) – Section 11.

---

## Cấu trúc thư mục

```
c/
├── README.md              ← Bạn đang ở đây (C core)
└── embedded.md            ← Lập trình nhúng (STM32, AVR, ESP32, RTOS)
```

---

## Mục lục

- [1. Rules template cho C](#1-rules-template-cho-c)
- [2. Prompt Library cho C](#2-prompt-library-cho-c)
- [3. Debug Prompts](#3-debug-prompts)
- [4. Build System](#4-build-system)
- [5. Testing Prompts](#5-testing-prompts)
- [6. Best Practices & Anti-patterns](#6-best-practices--anti-patterns)

---

## 1. Rules template cho C

### 1.1 File `.cursor/rules/c-lang.mdc`

```
---
description: Rules for C language files
globs: *.c, *.h
---

- Write C11/C17 standard-compliant code
- Check all malloc/calloc/realloc return values for NULL
- Free all dynamically allocated memory – no leaks
- Check pointers for NULL before dereferencing
- Use const for read-only parameters and pointers
- Never use gets(), sprintf(), strcpy() – use safe variants
- Check all return values of system calls
- Use include guards in all header files
- Follow existing naming convention (snake_case typical for C)
- Keep functions < 50 lines, single responsibility
- Use goto cleanup pattern for error handling with multiple resources
```

### 1.2 Full `.cursorrules` cho C project

```txt
You are an expert C programmer with deep knowledge of systems programming,
memory management, and performance optimization.

# General
- Write C11/C17 standard-compliant code.
- Follow existing coding style. Keep functions short and focused.
- Do NOT introduce new libraries unless explicitly asked.

# Memory Management
- Always free dynamically allocated memory – no leaks.
- Check return value of malloc/calloc/realloc – handle NULL.
- Document ownership of allocated memory (who allocates, who frees).
- Prefer stack allocation over heap when feasible.

# Pointer Safety
- Always check pointers for NULL before dereferencing.
- Never return pointers to local variables.
- Use const pointers when data should not be modified.

# Error Handling
- Always check return values of system calls and library functions.
- Use goto cleanup pattern for multi-resource cleanup.
- Log errors with context.

# Security
- Never use gets(), sprintf(), strcpy().
- Validate all input sizes before buffer operations.
- Initialize all variables before use.

# Output Format
- Always start with: "Files changed/created:"
- Use code blocks with 'c' language identifier.
```

---

## 2. Prompt Library cho C

### 2.1 Tạo module mới

```text
@codebase

Create a new C module for [mô tả chức năng].

Requirements:
- Header file (.h) with public API and documentation
- Implementation file (.c) with static helper functions
- Include guards in header
- Proper error handling (return codes)
- Memory management: document who allocates/frees
- All functions documented with comments
```

### 2.2 Tạo struct + functions

```text
@codebase

Create a struct for [mô tả data] with:
- Constructor (create/init), destructor (destroy/free)
- Getter/setter functions
- Print/debug function
- Opaque pointer pattern if hiding internals
- No memory leaks in any code path
```

### 2.3 Data structure implementation

```text
@codebase

Implement a [linked list / hash table / queue / stack / tree] in C:
- Generic data support (void* or macro-based)
- Create, destroy, insert, delete, search operations
- Proper memory management (no leaks)
- NULL checks on all pointer parameters
- Unit test examples
```

### 2.4 File I/O module

```text
@codebase

Create file I/O utility for [mô tả]:
- Open/close with proper error handling
- Read/write with buffer management
- Handle partial reads/writes
- Proper cleanup on error (fclose, free)
```

### 2.5 Refactor C code

```text
@file [filepath]

Refactor this C code:
- Eliminate code duplication
- Add proper error handling and cleanup
- Fix memory leaks or unsafe patterns
- Add const where appropriate
- Improve naming for clarity

Keep exact same behavior. Do not change public API.
```

---

## 3. Debug Prompts

### 3.1 Segmentation Fault

```text
@terminal
@file [filepath]

Segfault. Valgrind/ASAN output:
[paste output]

Please: identify crash cause, fix, add defensive checks.
```

### 3.2 Memory Leak

```text
@terminal
@file [filepath]

Valgrind --leak-check=full output:
[paste output]

Please: identify leaks, add free() on all paths including error paths.
```

### 3.3 Undefined Behavior

```text
@file [filepath]

Possible UB. Compiler warnings: [paste]

Please: identify UB sources, fix, suggest sanitizer flags.
```

### 3.4 Compiler Error

```text
@terminal
@file [filepath]

Compilation failed: [paste error]

Fix errors without changing intended logic.
```

---

## 4. Build System

### 4.1 Makefile

```text
@codebase

Create Makefile with targets: all, clean, debug, release, test
- Debug: -g -O0 -fsanitize=address,undefined
- Release: -O2 -DNDEBUG
- Warnings: -Wall -Wextra -Werror -Wpedantic
- Automatic dependency tracking (.d files)
```

### 4.2 CMakeLists.txt

```text
@codebase

Create CMakeLists.txt:
- C11/C17 standard
- Debug/Release build types
- Sanitizers in Debug
- External libraries: [list]
- CTest integration
```

---

## 5. Testing Prompts

### 5.1 Unit tests

```text
@codebase

Write unit tests for @file [filepath]:
- Framework: [CUnit / Unity / Check / CMocka]
- Test each public function
- Edge cases: NULL, empty, max values, overflow
- Memory leak checks
```

---

## 6. Best Practices & Anti-patterns

### ✅ DO
- Check NULL trước dereference | Free memory trên mọi code path
- `const` cho read-only | `static` cho file-scope | `size_t` cho sizes
- `snprintf` thay `sprintf` | `strncpy` thay `strcpy`
- Compile với `-Wall -Wextra -Werror` | Chạy Valgrind/ASAN thường xuyên

### ❌ DON'T
- Không `gets()`, `sprintf()`, `strcpy()` | Không ignore compiler warnings
- Không return pointer tới local variable | Không cast malloc result trong C
- Không magic numbers – dùng `#define` hoặc `enum`
- Không nested > 3 levels

---

## Xem thêm

| Chủ đề | File |
|--------|------|
| **Lập trình nhúng (Embedded)** | [embedded.md](./embedded.md) |

> 📌 Quay lại [Handbook chính](../cursor-ide-handbook.md)
