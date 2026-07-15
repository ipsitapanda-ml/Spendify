# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project overview

Spendify is a Flask-based expense tracker built incrementally as a step-by-step learning project. Route handlers and templates are already scaffolded; several routes and the entire database layer are intentionally left as stubs for the student to implement ("Step N" comments mark unfinished work — see `app.py` and `database/db.py`).

## Commands

```bash
# Setup (from repo root)
python -m venv venv
venv\Scripts\activate          # Windows
pip install -r requirements.txt

# Run the dev server (http://localhost:5001)
python app.py

# Run tests
pytest
```

No test files exist yet (`pytest-flask` is installed as a dependency but there is no `tests/` directory). No linter/formatter is configured.

## Architecture

- **`app.py`** — single Flask application instance with all routes. Fully-implemented routes render templates directly (`landing`, `register`, `login`, `terms`, `privacy`). Unfinished routes (`logout`, `profile`, `expenses/add`, `expenses/<id>/edit`, `expenses/<id>/delete`) currently return placeholder strings and are meant to be built out alongside the database layer.
- **`database/db.py`** — intended to hold `get_db()` (SQLite connection with `row_factory` and foreign keys enabled), `init_db()` (creates tables with `CREATE TABLE IF NOT EXISTS`), and `seed_db()` (sample data for dev). Not yet implemented — this is the next major piece of work before the expense routes can function.
- **`templates/base.html`** — shared layout (nav, footer) that all pages extend via Jinja `{% block %}`. Other templates (`landing.html`, `login.html`, `register.html`, `terms.html`, `privacy.html`) extend this base.
- **`static/css/style.css`** — single stylesheet for the whole app (no build step/bundler).
- **`static/js/main.js`** — currently empty; intended location for future client-side JS.
- **`render.yaml`** — Render.com deployment config; runs via `gunicorn --bind 0.0.0.0:$PORT app:app`.
- SQLite database file (`expense_tracker.db`) is gitignored and created locally by `init_db()`/`seed_db()` once implemented.

## Notes

- This is a solo learning project, not a production app — expect intentionally incomplete routes and no auth/session handling yet.
- When implementing the database layer or new routes, follow the contract already documented in the `database/db.py` docstring rather than inventing a different schema/API shape.
