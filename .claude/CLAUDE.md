# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Spendify is a Flask-based expense tracker built incrementally as a step-by-step learning project. Route handlers and templates are already scaffolded; several routes and the entire database layer are intentionally left as stubs for the student to implement ("Step N" comments mark unfinished work — see `app.py` and `database/db.py`).

## Architecture

- **`app.py`** — single Flask application instance with all routes.
- **`database/db.py`** — intended to hold `get_db()` (SQLite connection with `row_factory` and foreign keys enabled), `init_db()` (creates tables with `CREATE TABLE IF NOT EXISTS`), and `seed_db()` (sample data for dev). Not yet implemented — this is the next major piece of work before the expense routes can function.
- **`templates/base.html`** — shared layout (nav, footer) that all pages extend via Jinja `{% block %}`. Other templates (`landing.html`, `login.html`, `register.html`, `terms.html`, `privacy.html`) extend this base.
- **`static/css/style.css`** — single stylesheet for the whole app (no build step/bundler).
- **`static/js/main.js`** — currently empty; intended location for future client-side JS.
- **`render.yaml`** — Render.com deployment config; runs via `gunicorn --bind 0.0.0.0:$PORT app:app`.
- SQLite database file (`expense_tracker.db`) is gitignored and created locally by `init_db()`/`seed_db()` once implemented.

## Code Style

- Routes in `app.py` are grouped under banner comments (e.g. `# --- Routes --- #`); keep new routes under the appropriate banner rather than scattering them.
- Route function names are snake_case and match the route's purpose (`add_expense`, `edit_expense`), not the URL literally.
- One route per function, minimal/no docstrings — behavior is expected to be obvious from the function and route name.
- Templates extend `base.html` via Jinja `{% block %}` inheritance; don't duplicate the nav/footer markup in child templates.
- CSS is a single hand-written stylesheet — no preprocessor, framework, or CSS-in-JS.
- No linter or formatter is configured in this repo; match existing file conventions rather than an enforced tool.

## Tech Constraints

- Dependency versions are pinned in `requirements.txt`: flask 3.1.3, werkzeug 3.1.6, gunicorn 23.0.0, pytest 8.3.5, pytest-flask 1.3.0.
- No Node/JS build tooling — vanilla JS and CSS only, served directly from `static/`.
- Database is SQLite via the stdlib `sqlite3` module (per the intended contract in `database/db.py`), not an ORM.
- Deployment target is Render.com, free plan, via `render.yaml` (`gunicorn --bind 0.0.0.0:$PORT app:app`).
- Dev server runs on port 5001 (`python app.py`), not Flask's default 5000.

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

No test files exist yet (`pytest-flask` is installed as a dependency but there is no `tests/` directory).

## Implemented vs Stub Routes

**Implemented** (render templates):
- `GET /` — landing
- `GET /register` — register
- `GET /login` — login
- `GET /terms` — terms
- `GET /privacy` — privacy

**Stub** (return placeholder strings only, not yet built):
- `GET /logout` — Step 3
- `GET /profile` — Step 4
- `GET /expenses/add` — Step 7
- `GET /expenses/<int:id>/edit` — Step 8
- `GET /expenses/<int:id>/delete` — Step 9

`database/db.py` (`get_db()`, `init_db()`, `seed_db()`) is also entirely unimplemented and blocks all of the expense routes above from functioning.

## Warnings and Things to Avoid

- Don't invent a different DB schema or API shape than what's already documented in the `database/db.py` docstring.
- No auth/session handling exists yet — don't assume an authenticated user in new routes or templates.
- Never commit `expense_tracker.db` (already gitignored) — it's a local dev artifact created by `init_db()`/`seed_db()`.
- Don't add tests assuming existing test infrastructure — there is none yet (`pytest` is installed but unused).
- This is a solo learning project, not a production app — avoid over-engineering the stub routes or adding abstractions ahead of what each "Step" requires.
