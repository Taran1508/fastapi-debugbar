### fastapi-debugbar – Design Blueprint

### Overview

A modern, async-safe debug toolbar for FastAPI, inspired by Django Debug Toolbar but built for the ASGI/async ecosystem. It provides request/response details, SQLAlchemy query tracking, and an extensible plugin system.

### Goals

- **Async-safe**: Use `contextvars` for request-scoped storage.
- **SQLAlchemy 2.x ready**: Full support for the latest SQLAlchemy async/sync events.
- **Plugin-first**: Panels are pluggable; third-party developers can add custom metrics.
- **Production-safe by default**: Disabled unless explicitly enabled in development or via secure token.
- **Modern Python packaging**: PEP 621 (`pyproject.toml`), GitHub Actions CI/CD, type hints.

### Core Architecture

- **Middleware Layer**
  - An ASGI middleware that wraps each request.
  - Starts/ends timing, initializes context, calls next app.
  - Collects final response, injects toolbar (HTML overlay) or exposes `/__debug__` JSON endpoint.
- **Panels (Plugins)**
  - Each panel implements:
    - `on_request_start(scope, receive, send)`
    - `on_request_end(scope, response)`
    - `render_html()` or `render_json()`
  - Core panels planned:
    1. Request/Response info
    2. SQLAlchemy queries
    3. Execution timing
    4. Profiler (optional, toggle)
    5. Templates (future)
- **Adapters**
  - For ORMs and DB drivers (start with SQLAlchemy).
  - Register via events to record queries.
- **Configuration**
  - `enabled`: callable or bool.
  - `panels`: list of panel classes.
  - `redact_headers`: list of sensitive headers.
  - `max_body_size`: int (bytes).

### Security

- Disabled by default in production.
- Requires explicit opt-in (`DEBUG=True` or `DEBUG_TOKEN`).
- Redacts sensitive data by default.

### Future Roadmap

- Async ORM adapters (Tortoise, Gino, Prisma).
- Frontend widget with toggle.
- Exportable debug sessions.
