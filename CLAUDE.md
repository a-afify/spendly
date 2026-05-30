# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project

Spendly — a Flask expense-tracking web app. Python backend, Jinja2 templates, vanilla JS/CSS. No build step.

## Running the app

```bash
# First-time setup
python -m venv venv
venv\Scripts\activate        # Windows
pip install -r requirements.txt

# Start dev server
python app.py                # http://localhost:5001
```

Debug mode is hardcoded on. Port is hardcoded to 5001.

## Testing

```bash
pytest
```

pytest and pytest-flask are in requirements. No test files exist yet.

## Project structure

- `app.py` — all Flask routes
- `templates/base.html` — Jinja2 base template; all pages extend it
- `database/db.py` — **skeleton only** (comments describe expected functions, no implementation yet)
- `database/__init__.py` — empty
- `static/js/main.js` — placeholder, not implemented

Do not assume `database/db.py` has working functions — it is a student implementation target.

## Placeholder routes

Several routes return plain strings ("coming in Step N"). These are intentional scaffolding:
- `/logout`, `/profile`, `/expenses/add`, `/expenses/<id>/edit`, `/expenses/<id>/delete`

## CSS design system

Custom properties are defined in `static/css/style.css` under `:root`. Use these tokens rather than hardcoded values:
- Colors: `--ink-*` (dark), `--paper-*` (light), `--accent` (green `#1a472a`), `--secondary` (orange `#c17f24`), `--danger`
- Radius: `--radius-sm` (6px), `--radius-md` (12px), `--radius-lg` (20px)
- Fonts: DM Serif Display (display), DM Sans (body) — loaded from Google Fonts

## Dependencies

werkzeug is already included (via Flask) and provides `werkzeug.security` for password hashing when auth is implemented.
