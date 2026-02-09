# 📘 CURSOR PROMPTS – Python Core

**Rules • Prompts • Debug • Best Practices cho Python trong Cursor IDE**

> Thư mục này là phần mở rộng của [cursor-ide-handbook.md](../cursor-ide-handbook.md) – Section 11.

---

## Cấu trúc thư mục

```
python/
├── README.md              ← Bạn đang ở đây (Python core)
├── web.md                 ← Flask, FastAPI, Django
├── data.md                ← NumPy, Pandas, Matplotlib, Data Engineering
└── ai.md                  ← RAG, LLM, Computer Vision, ML/DL
```

---

## Mục lục

- [1. Rules template cho Python](#1-rules-template-cho-python)
- [2. Prompt Library – Python Core](#2-prompt-library--python-core)
- [3. Debug Prompts](#3-debug-prompts)
- [4. Testing Prompts](#4-testing-prompts)
- [5. Tooling & Automation](#5-tooling--automation)
- [6. Best Practices & Anti-patterns](#6-best-practices--anti-patterns)

---

## 1. Rules template cho Python

### 1.1 File `.cursorrules` cho Python project

```txt
You are an expert Python developer with deep knowledge of Python 3.12+,
type hints, async programming, and Python best practices.

# General Python Rules
- Write modern Python (3.11+ minimum, 3.12 preferred).
- Follow PEP 8 style guide.
- Follow existing project structure and naming conventions.
- Prefer readability and simplicity (Zen of Python).
- Do NOT introduce new pip packages unless explicitly asked.

# Type Hints
- Add type hints to ALL function signatures (parameters + return type).
- Use modern syntax: list[str], dict[str, int], str | None (not Optional[str]).
- Use TypeAlias, TypeVar, Protocol, Generic for complex types.
- Use @dataclass or Pydantic BaseModel for data structures.
- Run mypy-compatible code.

# Naming Conventions
- snake_case for: functions, methods, variables, modules.
- PascalCase for: classes, type aliases, exceptions.
- UPPER_SNAKE_CASE for: constants.
- _private for: internal/private attributes.

# Docstrings
- Google style or NumPy style docstrings (pick one, be consistent).
- Docstrings for all public functions, classes, modules.
- Include: description, Args, Returns, Raises.

# Error Handling
- Use specific exception types (ValueError, TypeError, etc.).
- Create custom exceptions for domain-specific errors.
- Never use bare except: or except Exception:.
- Use contextlib.suppress() for expected exceptions.

# Security
- Never use eval() or exec() with user input.
- Use parameterized queries for SQL.
- Validate and sanitize all user input.
- Use secrets module for random tokens (not random).

# Output Format
- Always start with: "Files changed/created:"
- List each file path clearly.
- Use code blocks with 'python' language identifier.
```

### 1.2 File `.cursor/rules/python.mdc`

```
---
description: Rules for Python files
globs: *.py
---

- Write modern Python 3.11+ code
- Follow PEP 8 style guide
- Type hints on ALL function signatures: params + return type
- Use modern type syntax: list[str], str | None (not Optional)
- Use @dataclass or Pydantic for data structures
- Docstrings (Google style) for all public functions/classes
- Use specific exception types, not bare except
- Use f-strings for string formatting
- Use pathlib.Path instead of os.path
- Use logging module instead of print()
- Keep functions short, single responsibility
```

---

## 2. Prompt Library – Python Core

### 2.1 Tạo module mới

```text
@codebase

Create a Python module for [mô tả chức năng].

Requirements:
- Proper module structure (__init__.py if package)
- Type hints for all functions
- Google-style docstrings
- Logging instead of print()
- Custom exceptions if needed
- Example usage in docstring or __main__ block
- Constants at module level (UPPER_SNAKE_CASE)

Follow existing project structure.
```

### 2.2 Tạo class / dataclass

```text
@codebase

Create a Python class for [mô tả].

Requirements:
- Use @dataclass (or Pydantic BaseModel if validation needed)
- Type hints for all fields
- __post_init__ for validation (dataclass) or @validator (Pydantic)
- Properties for computed fields
- __str__ / __repr__ for debugging
- Comparison methods if needed (__eq__, __lt__)
- Factory methods (classmethods) for alternative constructors
- Docstring with usage example
```

### 2.3 Tạo CLI tool

```text
@codebase

Create a CLI tool for [mô tả chức năng].

Requirements:
- Use [argparse / click / typer]
- Subcommands if applicable
- Proper help messages for all options
- Input validation
- Error handling with user-friendly messages
- Exit codes (0 success, 1 error)
- Logging with --verbose flag
- Progress bars for long operations (tqdm/rich)
```

### 2.4 Implement design pattern

```text
@codebase

Implement the [Singleton / Factory / Observer / Strategy / Repository / Decorator] pattern in Python.

Requirements:
- Pythonic implementation (use Python features, not Java-style)
- Type hints and generics where appropriate
- Protocol classes for interfaces (structural typing)
- Dataclasses for value objects
- ABC for abstract base classes (if needed)
- Example usage and unit test
```

### 2.5 Refactor Python code

```text
@file [filepath]

Refactor this Python code:
- Add type hints to all functions
- Add docstrings to public functions
- Replace print() with logging
- Use pathlib.Path instead of os.path
- Use f-strings instead of .format() or %
- Use list comprehension where appropriate
- Extract long functions into smaller ones
- Replace magic numbers with named constants
- Use dataclass/Pydantic for data structures

Keep exact same behavior.
```

### 2.6 Async implementation

```text
@codebase

Create an async module for [mô tả: HTTP client, file processing, database access].

Requirements:
- async/await throughout
- Use asyncio.gather() for concurrent operations
- Proper exception handling in async context
- Graceful shutdown (handle CancelledError)
- Timeout handling
- Connection pooling if applicable
- Type hints for all async functions
```

### 2.7 Context manager

```text
@codebase

Create a context manager for [mô tả: database connection, file lock, temporary directory, timer].

Requirements:
- Implement both sync (__enter__/__exit__) and async (__aenter__/__aexit__)
- Or use @contextmanager / @asynccontextmanager decorator
- Proper cleanup on exception
- Type hints
- Usage example in docstring
```

---

## 3. Debug Prompts

### 3.1 Traceback Error

```text
@terminal
@file [filepath]

Python traceback:
[paste full traceback]

Please:
- Read the full traceback from bottom to top
- Identify root cause
- Fix the bug
- Add proper exception handling
```

### 3.2 Type Error

```text
@terminal
@file [filepath]

TypeError:
[paste error]

Please:
- Identify the type mismatch
- Fix with proper type handling / conversion
- Add type hints if missing
- Suggest running mypy to catch similar issues
```

### 3.3 Import Error

```text
@terminal
@codebase

ImportError / ModuleNotFoundError:
[paste error]

Please:
- Check if the module is installed (pip list)
- Check for circular imports
- Verify __init__.py files
- Fix the import structure
```

### 3.4 Async Bug

```text
@terminal
@file [filepath]

Async error:
[paste error: RuntimeWarning, never awaited, event loop already running, etc.]

Please:
- Identify the async issue
- Fix the async/await chain
- Ensure no blocking calls in async context
```

### 3.5 Memory Issue

```text
@codebase

Memory issue:
[mô tả: OOM, memory growing, memory leak]

Please:
- Identify memory-hungry operations
- Fix leaks (generator instead of list, chunking, del unused objects)
- Use gc.collect() if circular references suspected
```

### 3.6 Package / Environment Issue

```text
@terminal

Environment issue:
[paste: pip error, version conflict, venv issue, poetry lock error]

Please:
- Diagnose the package/environment issue
- Show exact commands to fix
- Update requirements.txt / pyproject.toml if needed
```

---

## 4. Testing Prompts

### 4.1 Unit tests với pytest

```text
@codebase

Write pytest tests for @file [filepath].

Requirements:
- pytest + pytest-asyncio (if async)
- Test each public function/method
- Fixtures for setup/teardown
- Parametrize for multiple test cases
- Mock external dependencies (pytest-mock / unittest.mock)
- Test happy path, edge cases, exceptions
- Naming: test_[function]_[scenario]_[expected]
- Use tmp_path for file operations
```

### 4.2 Integration tests

```text
@codebase

Write integration tests for [module/API].

Requirements:
- Test with real (test) database / services
- Fixture for test database setup/teardown
- Test full request/response cycle
- Use factories (factory_boy) for test data
```

### 4.3 Fixture library

```text
@codebase

Create a pytest fixture library for this project.

Required fixtures:
- Database session (with transaction rollback)
- Authenticated client
- Test data factories
- Temporary files/directories
- Mock external services
- Configuration overrides

Put in conftest.py at appropriate levels.
```

### 4.4 Property-based testing

```text
@codebase

Write property-based tests for [function/module] using Hypothesis.

Requirements:
- Define strategies for input generation
- Test invariants and properties (not specific values)
- @given decorator with appropriate strategies
- Shrink failing cases
```

---

## 5. Tooling & Automation

### 5.1 pyproject.toml setup

```text
@codebase

Create/update pyproject.toml for this project.

Requirements:
- Build system: [setuptools / poetry / hatch / flit]
- Project metadata
- Dependencies with version constraints
- Optional dependencies (dev, test, docs)
- Tool config: [tool.pytest], [tool.mypy], [tool.ruff]
- Scripts / entry points
```

### 5.2 Pre-commit hooks

```text
@codebase

Set up pre-commit hooks for this Python project.

Hooks:
- ruff (linting + formatting) or black + isort
- mypy (type checking)
- pytest (run fast tests)
- detect-secrets

Create .pre-commit-config.yaml.
```

### 5.3 Docker setup

```text
@codebase

Create Dockerfile for this Python project.

Requirements:
- Multi-stage build
- Python 3.12 slim base
- Non-root user
- Pip install from requirements.txt
- Health check
- Proper COPY order for caching
- .dockerignore + docker-compose.yml
```

---

## 6. Best Practices & Anti-patterns

### ✅ DO

- Dùng type hints cho mọi function signature
- Dùng f-strings: `f"Hello {name}"`
- Dùng `pathlib.Path` thay vì `os.path`
- Dùng `logging` module thay vì `print()`
- Dùng `dataclass` hoặc Pydantic cho data structures
- Dùng `enum.Enum` cho fixed sets
- Dùng context managers (`with` statement)
- Dùng `functools.lru_cache` cho memoization
- Dùng `typing.Protocol` cho structural typing
- Dùng virtual environment cho mọi project

### ❌ DON'T

- Không dùng mutable default arguments: `def f(lst=[])` → dùng `None`
- Không dùng bare `except:` → catch specific exceptions
- Không dùng `eval()` / `exec()` với user input
- Không dùng `import *`
- Không dùng `global` keyword
- Không dùng `print()` cho production logging
- Không dùng `os.path` – dùng `pathlib`
- Không dùng `range(len(lst))` – dùng `enumerate(lst)`
- Không commit `.env` files hoặc secrets

---

## Xem thêm

| Chủ đề | File |
|--------|------|
| **Web (Flask, FastAPI, Django)** | [web.md](./web.md) |
| **Data (NumPy, Pandas, Matplotlib)** | [data.md](./data.md) |
| **AI (RAG, LLM, Computer Vision)** | [ai.md](./ai.md) |

> 📌 Quay lại [Handbook chính](../cursor-ide-handbook.md)
