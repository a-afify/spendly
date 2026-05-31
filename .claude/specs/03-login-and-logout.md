# Spec: Login and Logout

## Overview

Implement session-based login and logout so registered users can authenticate with their email and password and access protected areas of Spendly. This step upgrades the existing `/login` GET stub to handle POST submissions, verifies credentials against the database, stores the authenticated user's ID in a Flask session, and redirects to a dashboard placeholder. Logout clears the session and redirects to the landing page. It establishes the authentication pattern that all future protected routes (profile, expenses) will depend on.

## Depends on

- Step 01 — Database Setup (`get_db` must be working)
- Step 02 — Registration (users must exist in the `users` table)

## Routes

- `GET /login` — render the login form — public
- `POST /login` — validate credentials, start session, redirect to `/dashboard` on success — public
- `GET /logout` — clear the session, redirect to `/` — logged-in (no enforcement yet, redirect works for any visitor)

## Database changes

No new tables or columns. A new helper function `get_user_by_email(email)` must be added to `database/db.py` to look up a user row by email for credential verification.

## Templates

- **Modify**: `templates/login.html` — add a `<form method="POST">` with email and password fields; display inline error messages
- **Create**: `templates/dashboard.html` — minimal logged-in landing page showing "Welcome, {name}" and a logout link; extends `base.html`

## Files to change

- `app.py` — convert `login()` to handle GET and POST; implement `logout()` route; store `user_id` and `user_name` in `flask.session`; import `get_user_by_email` and `check_password_hash`
- `database/db.py` — add `get_user_by_email(email)` function
- `templates/login.html` — add the POST form and error display

## Files to create

- `templates/dashboard.html` — post-login landing page

## New dependencies

No new dependencies. `werkzeug.security.check_password_hash` is already available via Flask's dependency on Werkzeug.

## Rules for implementation

- No SQLAlchemy or ORMs
- Parameterised queries only — never string-format SQL
- Verify passwords with `werkzeug.security.check_password_hash`
- Use CSS variables — never hardcode hex values
- All templates extend `base.html`
- Store only `user_id` and `user_name` in `flask.session` — never store the password hash
- On failed login show an inline error (not a flash); do not reveal whether the email or password was wrong — always say "Invalid email or password"
- `app.secret_key` is already set in `app.py` — do not change it
- `logout()` must call `session.clear()` before redirecting

## Definition of done

- [ ] Visiting `/login` renders a form with email and password fields
- [ ] Submitting valid credentials starts a session and redirects to `/dashboard`
- [ ] `/dashboard` displays "Welcome, {name}" with the logged-in user's name
- [ ] Submitting an incorrect password shows "Invalid email or password" and re-renders the form
- [ ] Submitting an email that does not exist shows "Invalid email or password" and re-renders the form
- [ ] Visiting `/logout` clears the session and redirects to `/`
- [ ] After logout, visiting `/dashboard` does not show the previous user's name (session is gone)
- [ ] The login page is styled using CSS variables and matches the Spendly design system
