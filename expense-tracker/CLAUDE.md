# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project status

This is a Flask learning project in an early, intentionally incomplete scaffold state ("Spendly" — a personal expense tracker). Code comments mark unimplemented pieces as steps in a build sequence (e.g. "Step 1 — Database Setup", "coming in Step 3/4/7/8/9"). When asked to implement a feature, check the relevant file's comments first for the intended scope/order before adding code.

Current state:
- `app.py` — Flask app with real routes for `/`, `/register`, `/login` (render templates only, no POST handling yet) and placeholder routes for `/logout`, `/profile`, `/expenses/add`, `/expenses/<id>/edit`, `/expenses/<id>/delete` that just return plain strings.
- `database/db.py` — empty stub; intended to hold `get_db()` (SQLite connection with `row_factory` and foreign keys enabled), `init_db()` (create tables with `CREATE TABLE IF NOT EXISTS`), and `seed_db()` (sample data for dev).
- `database/__init__.py` — empty.
- No database file, models, or auth logic exist yet — registration/login forms post to `/register` and `/login` but there is no server-side handling of the POST body.
- No tests exist yet despite `pytest`/`pytest-flask` being in `requirements.txt`.

## Commands

```bash
# setup
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt

# run the app (http://localhost:5001, debug mode on)
python app.py

# run tests (once a tests/ dir exists)
pytest
pytest path/to/test_file.py::test_name   # single test
```

The SQLite database file is `expense_tracker.db` at the project root (gitignored, created by `init_db()` once implemented).

## Architecture

- Standard Flask app with server-rendered Jinja2 templates (no API/JSON layer, no frontend framework).
- `templates/base.html` is the shared layout (nav, footer, font/CSS includes) — all pages `{% extends "base.html" %}` and fill `title`/`content`/`head`/`scripts` blocks.
- Templates use `url_for('<endpoint>')` to link between routes, so Flask endpoint function names (`landing`, `register`, `login`, etc. in `app.py`) are the source of truth for routing, not raw paths.
- `database/db.py` is meant to be the single place SQL/connection logic lives; routes in `app.py` should call into it rather than opening `sqlite3` connections directly.
- Styling is a single stylesheet (`static/css/style.css`) driven by CSS custom properties defined in `:root` (colors, fonts, spacing/radius scale) — reuse these variables rather than hardcoding new values.
- `static/js/main.js` is currently empty; it's the intended home for any client-side behavior.
