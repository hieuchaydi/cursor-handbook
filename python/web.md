# 📘 CURSOR PROMPTS – Python Web (Flask, FastAPI, Django)

**Rules • Prompts • Debug cho Python Web Frameworks trong Cursor IDE**

> File này thuộc thư mục [python/](./README.md).

---

## Mục lục

- [1. Flask](#1-flask)
- [2. FastAPI](#2-fastapi)
- [3. Django](#3-django)
- [4. Debug chung cho Python Web](#4-debug-chung-cho-python-web)

---

## 1. Flask

### 1.1 Rules cho Flask

```
---
description: Rules for Flask application files
globs: app/**/*.py, *.py
---

- Use Flask Blueprints for modular organization
- Use Flask-SQLAlchemy for ORM
- Use Flask-Migrate (Alembic) for database migrations
- Use Flask-WTF or marshmallow for validation
- Use application factory pattern (create_app())
- Use environment variables for configuration (python-dotenv)
- Proper error handlers (@app.errorhandler)
- Use Flask-Login or Flask-JWT-Extended for auth
- Type hints for all route handlers
- Return proper HTTP status codes and JSON responses
```

### 1.2 Tạo Flask app (Application Factory)

```text
@codebase

Create a Flask application with factory pattern:

Structure:
app/
├── __init__.py          (create_app factory)
├── config.py            (Config classes)
├── models/              (SQLAlchemy models)
├── routes/              (Blueprints)
├── services/            (Business logic)
├── schemas/             (Marshmallow / validation)
├── utils/               (Helpers)
└── extensions.py        (db, migrate, login_manager)

Requirements:
- create_app() factory with config parameter
- Blueprint registration
- Extension initialization (SQLAlchemy, Migrate, CORS)
- Error handlers (400, 404, 500)
- Logging configuration
- Environment-based config (Development/Production/Testing)
```

### 1.3 Flask Blueprint + CRUD

```text
@codebase

Create a Flask Blueprint for [resource] with CRUD endpoints:

Files:
- routes/[resource].py (Blueprint with routes)
- models/[resource].py (SQLAlchemy model)
- schemas/[resource].py (Marshmallow schema)
- services/[resource]_service.py (Business logic)

Endpoints:
- GET /api/[resources] – List with pagination
- GET /api/[resources]/<id> – Detail
- POST /api/[resources] – Create
- PUT /api/[resources]/<id> – Update
- DELETE /api/[resources]/<id> – Delete

Requirements:
- Input validation with Marshmallow
- Proper error handling and HTTP status codes
- Pagination (page, per_page, total)
- Type hints
- Docstrings
```

### 1.4 Flask Authentication

```text
@codebase

Implement authentication for Flask app:

Requirements:
- Use [Flask-Login / Flask-JWT-Extended]
- Registration endpoint (password hashing with bcrypt/werkzeug)
- Login endpoint (return JWT or set session)
- Protected route decorator (@login_required or @jwt_required)
- Refresh token flow (if JWT)
- User model with password hashing
- Role-based access control
```

### 1.5 Flask + SQLAlchemy Model

```text
@codebase

Create SQLAlchemy model for [entity]:

Requirements:
- Model class extending db.Model
- Proper column types, nullable, defaults
- Relationships (ForeignKey, relationship, backref)
- __repr__ method
- to_dict() method for serialization
- Indexes for frequently queried fields
- Created_at, updated_at timestamps
- Soft delete support (optional)

Create migration: flask db migrate -m "[description]"
```

### 1.6 Flask Testing

```text
@codebase

Write tests for Flask [Blueprint / endpoint]:

Requirements:
- pytest with Flask test client (app.test_client())
- conftest.py with app fixture (test config)
- Test database (in-memory SQLite or test DB)
- Test each endpoint (status codes, response body)
- Test authentication
- Test validation errors
- Fixture for authenticated client
```

---

## 2. FastAPI

### 2.1 Rules cho FastAPI

```
---
description: Rules for FastAPI application files
globs: app/**/*.py, *.py
---

- Use Pydantic v2 models for request/response validation
- Async route handlers (async def)
- Use dependency injection (Depends())
- Use APIRouter for modular organization
- Use proper HTTP status codes
- Use HTTPException for errors
- Type-safe path/query/body parameters
- OpenAPI documentation (summary, description, response_model)
- Use background tasks for heavy processing
- Use middleware for cross-cutting concerns
```

### 2.2 Tạo FastAPI endpoint đầy đủ

```text
@codebase

Create FastAPI endpoints for [resource]:

Files:
- routers/[resource].py (APIRouter)
- schemas/[resource].py (Pydantic models)
- services/[resource]_service.py (Business logic)
- models/[resource].py (SQLAlchemy/Tortoise model)
- dependencies.py (Depends functions)

Endpoints:
- GET /api/v1/[resources] – List with pagination/filter
- GET /api/v1/[resources]/{id} – Detail
- POST /api/v1/[resources] – Create
- PUT /api/v1/[resources]/{id} – Update
- DELETE /api/v1/[resources]/{id} – Delete

Requirements:
- Pydantic v2 request/response models
- Async handlers
- Dependency injection
- Proper HTTP status codes + error responses
- OpenAPI documentation (tags, summary, responses)
- Type hints throughout
```

### 2.3 FastAPI Pydantic Models

```text
@codebase

Create Pydantic v2 models for [entity]:

Requirements:
- BaseModel with model_config
- Request model (Create/Update) with Field() validation
- Response model with only exposed fields
- Custom validators (@field_validator, @model_validator)
- Nested models for complex data
- Examples in model_config (json_schema_extra)
- Type coercion where needed
```

### 2.4 FastAPI Dependencies

```text
@codebase

Create FastAPI dependencies for [auth / pagination / database / rate limiting]:

Requirements:
- Depends() function or class
- Async support
- Proper type hints
- Error handling (raise HTTPException)
- Cacheable if appropriate
- Unit testable (overridable)
```

### 2.5 FastAPI WebSocket

```text
@codebase

Create WebSocket endpoint for [real-time feature]:

Requirements:
- WebSocket route
- Connection manager (connect, disconnect, broadcast)
- Authentication
- Message validation (Pydantic)
- Room/channel support
- Heartbeat / ping-pong
- Error handling
```

### 2.6 FastAPI + SQLAlchemy 2.0 Async

```text
@codebase

Create async database layer with SQLAlchemy 2.0:

Requirements:
- AsyncSession + async_sessionmaker
- Declarative models (Mapped, mapped_column)
- Async repository/service pattern
- Alembic async migrations
- Transactions
- Connection pooling
- Type-safe queries
```

### 2.7 FastAPI Background Tasks + Celery

```text
@codebase

Set up background processing:

Option 1 (simple): FastAPI BackgroundTasks
Option 2 (robust): Celery + Redis/RabbitMQ

Requirements:
- Task definition with typing
- Retry logic
- Error handling
- Result storage
- Task monitoring
- Periodic tasks (Celery Beat)
```

---

## 3. Django

### 3.1 Rules cho Django

```
---
description: Rules for Django application files
globs: */models.py, */views.py, */serializers.py, */urls.py, */admin.py, */forms.py, */tests.py
---

- Follow Django conventions (MTV pattern)
- Use class-based views (CBV) when appropriate
- Use Django ORM properly (avoid N+1, use select_related/prefetch_related)
- Use Django REST Framework for APIs
- Use Django forms/serializers for validation
- Migrations for all model changes
- Use Django signals sparingly
- Use Django admin for quick management interfaces
- Proper settings management (django-environ or django-configurations)
- Type hints where possible (Django-stubs)
```

### 3.2 Django Model + Views đầy đủ

```text
@codebase

Create Django model and views for [entity]:

Files:
- models.py (Model)
- views.py (CBV or ViewSet)
- serializers.py (DRF Serializer)
- urls.py (URL patterns)
- admin.py (Admin registration)
- tests.py (Tests)

Requirements:
- Model with proper fields, Meta class, __str__
- Choices with TextChoices/IntegerChoices
- Custom Manager with common queries
- Signals if needed (post_save, etc.)
- Migration
```

### 3.3 Django REST Framework API

```text
@codebase

Create DRF API for [resource]:

Requirements:
- ModelSerializer with validation
- ViewSet or APIView
- Router registration
- Permission classes (IsAuthenticated, custom)
- Filtering (django-filter) + Search + Ordering
- Pagination (PageNumberPagination)
- Nested serializers for relations
- Custom actions (@action)
- Throttling configuration
```

### 3.4 Django Admin Customization

```text
@codebase

Customize Django admin for [model]:

Requirements:
- list_display, list_filter, search_fields
- Inline models for related objects
- Custom actions (export, bulk update)
- Fieldsets for organized edit view
- Read-only fields
- Auto-complete fields for FK
- Custom queryset (get_queryset)
```

### 3.5 Django Management Command

```text
@codebase

Create management command for [task: import, cleanup, report]:

Requirements:
- BaseCommand with add_arguments, handle
- --dry-run option
- Progress indication
- Logging
- Transaction support
- Error handling with proper exit codes
```

### 3.6 Django Middleware + Signals

```text
@codebase

Create Django [middleware / signal] for [mô tả]:

Middleware:
- __call__ or process_request/process_response
- Async support (__acall__)
- Register in MIDDLEWARE setting

Signal:
- post_save / pre_save / post_delete etc.
- Separate signals.py
- Register in AppConfig.ready()
```

### 3.7 Django Channels (WebSocket)

```text
@codebase

Create Django Channels consumer for [real-time feature]:

Requirements:
- AsyncWebsocketConsumer
- Channel layer (Redis)
- Room/group management
- Authentication
- Message routing
- JSON serialization
- Error handling
```

### 3.8 Django Testing

```text
@codebase

Write tests for Django [model / view / API]:

Requirements:
- TestCase or APITestCase
- Factory Boy for test data
- Test CRUD operations
- Test permissions
- Test validation
- Test edge cases
- Database fixtures or factories
```

---

## 4. Debug chung cho Python Web

### 4.1 500 Internal Server Error

```text
@terminal
@file [filepath]

500 error:
[paste full traceback from logs]

Please:
- Identify root cause
- Fix the bug
- Add error handling to prevent recurrence
```

### 4.2 N+1 Query Problem

```text
@codebase
@file [filepath]

N+1 query issue (too many database queries):

Debug log:
[paste SQL queries or describe]

Please:
- Identify N+1 patterns
- Fix with:
  - Django: select_related / prefetch_related
  - SQLAlchemy: joinedload / subqueryload / selectinload
  - FastAPI/Prisma: include / join
- Verify fix reduces query count
```

### 4.3 CORS / Auth Error

```text
@terminal
@codebase

CORS or authentication error:
[paste error: 403, 401, CORS blocked, etc.]

Please:
- Check CORS configuration
- Check auth middleware/decorator
- Verify token handling
- Fix configuration
```

### 4.4 Migration Error

```text
@terminal
@codebase

Migration error:
[paste error output]

Please:
- Identify migration issue
- Fix migration file or model
- Check for data conflicts
- Verify rollback works
```

---

> 📌 Quay lại [Python Core](./README.md) | [Handbook chính](../cursor-ide-handbook.md) | Xem thêm: [Data](./data.md) | [AI](./ai.md)
