# Spec: Registration

## Overview

Implement user registration so new visitors can create a Spendly account. This step wires up the existing `/register` GET route to also handle POST submissions, validates the form input, inserts a new user into the `users` table with a hashed password, and redirects on success. It is the first user-facing feature that writes to the database and sets the pattern for all future form handling in the app.

## Depends on

- Step 01 — Database Setup (`get_db`, `init_db`, `seed_db` must be working)

## Routes

- `GET /register` — render the registration form — public
- `POST /register` — process the form submission, create the user, redirect to `/login` on success — public

## Database changes

No new tables or columns. The existing `users` table already has all required fields:
- `name` TEXT NOT NULL
- `email` TEXT UNIQUE NOT NULL
- `password_hash` TEXT NOT NULL
- `created_at` TEXT DEFAULT (datetime('now'))

A new helper function `create_user(name, email, password)` must be added to `database/db.py`.

## Templates

- **Modify**: `templates/register.html` — add a `<form method="POST">` with fields for name, email, password, and confirm password; display flash error messages

## Files to change

- `app.py` — convert `register()` to handle GET and POST; add form validation and flash messages
- `database/db.py` — add `create_user(name, email, password)` function
- `templates/register.html` — add the POST form and error display

## Files to create

None.

## New dependencies

No new dependencies.

## Rules for implementation

- No SQLAlchemy or ORMs
- Parameterised queries only — never string-format SQL
- Hash passwords with `werkzeug.security.generate_password_hash` before storing
- Use CSS variables — never hardcode hex values
- All templates extend `base.html`
- Use `flask.flash` + `redirect` for error and success messaging
- A `secret_key` must be set on the Flask `app` for sessions/flash to work
- Catch the `sqlite3.IntegrityError` that fires on duplicate email and show a user-friendly flash message
- Validate server-side: name non-empty, valid email format, password length ≥ 8, passwords match

## Definition of done

- [ ] Visiting `/register` renders a form with name, email, password, and confirm-password fields
- [ ] Submitting valid details creates a new row in the `users` table with a hashed (not plain-text) password
- [ ] After successful registration the user is redirected to `/login`
- [ ] Submitting a duplicate email shows a flash error and does not create a duplicate row
- [ ] Submitting mismatched passwords shows a flash error and re-renders the form
- [ ] Submitting an empty name or password shorter than 8 characters shows a flash error
- [ ] The page is styled using CSS variables and the form matches the Spendly design system