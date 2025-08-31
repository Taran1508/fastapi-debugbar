# fastapi-debugbar
A modern, lightweight debug toolbar for FastAPI — inspired by Django Debug Toolbar, but built for async-first FastAPI apps.

### Features

- Real-time request/response inspection
- SQLAlchemy query counting & timing (async + sync)
- Total execution time & endpoint resolution
- Developer-only by default (safe for production)
- Easy plugin system to add custom panels

---

### Installation

```bash
pip install fastapi-debugbar

```

---

### Quickstart

```python
from fastapi import FastAPI
from fastapi_debugbar import DebugBarMiddleware

app = FastAPI()
app.add_middleware(DebugBarMiddleware)

@app.get("/")
async def hello():
    return {"message": "DebugBar active!"}

```

Then open your app in the browser — you’ll see a debug toolbar overlay for each request (only in dev mode).

---

### Why fastapi-debugbar?

FastAPI lacks a built-in debug toolbar. Existing options are outdated or lack modern async support.

**fastapi-debugbar** — the modern, async-safe, plugin-based debug toolbar for FastAPI. Designed for SQLAlchemy 2.x and real-world dev/prod safety.

This library provides:

- **Async-first design** for FastAPI ≥0.100
- **Extensible panels**: add your own metrics easily
- **Production-safe toggle** (auto-disabled outside dev)

### 1. **Async-first & context-safe**

- Use **`contextvars`** for per-request storage → safe for async & background tasks.
- No global state pollution.

### 2. **Modern SQLAlchemy 2.x ready**

- Out-of-the-box support for SQLAlchemy 2.x event system (`before_cursor_execute` / `after_cursor_execute` for both sync & async).
- Clear roadmap for Tortoise ORM, asyncpg, or others.

### 3. **Plugin-first architecture**

- Define a **simple panel interface** so devs can write:
    
    ```python
    class MyPanel(BasePanel):
        async def on_request_start(self, request): ...
        async def on_request_end(self, request, response): ...
    
    ```
    
    and add it without forking.
    

### 4. **Production-safe by default**

- Only enabled when `DEBUG=true` or `X-Debug-Token` present.
- Redacts sensitive headers (e.g., Authorization, Cookies).
- Body capture is **size-limited** to prevent leaks.

### 5. **Modern packaging & CI**

- Uses `pyproject.toml` (PEP 621) from day one.
- Ships with GitHub Actions CI + PyPI Trusted Publishing.
- Type hints, Black, Ruff/Flake8 pre-commit hooks.

### 6. **Better DX (developer experience)**

- **Clear, beautiful docs** with real FastAPI examples (SQLAlchemy, ORM, no-ORM).
- JSON & HTML modes: developers can choose overlay or debug endpoint.
- Quickstart in 3 lines.

---

### Roadmap

- [x]  Request/response panel
- [x]  SQLAlchemy panel
- [ ]  Template rendering panel
- [ ]  Profiler panel
- [ ]  Cache hit/miss panel

---

### License

[MIT](./LICENSE)

---

 <!--### Contributing

Contributions are welcome! Please see CONTRIBUTING.md for guidelines. -->
