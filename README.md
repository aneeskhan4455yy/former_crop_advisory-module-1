# FarmAdvisor (Fieldwise) — Admin Dashboard

FarmAdvisor is a crop-intelligence admin dashboard for managing farmers, crops,
fertilizer guidance, and weather alerts. The frontend is a working prototype
built with static HTML/CSS/JS and Bootstrap 5, and it ships with a PHP + MySQL
backend that is ready to be wired up to power the same features with real data.

> **Status:** Module 1 (auth, admin shell, farmer management) is complete as a
> browser prototype. The PHP API in `api.php` already implements the session,
> password hashing, CSRF protection, and CRUD endpoints needed for the next
> integration step — connecting the frontend forms to real requests instead of
> in-memory demo data.

## Features

- **Admin login** with a session-based auth flow (demo credentials in the
  frontend today; real authentication is implemented server-side in `api.php`)
- **Dashboard overview** with quick stats and recent activity panels
- **Farmer management** — add farmers, view farmer profiles
- **Crop library** — add, edit, and browse crops with season, soil type, and
  watering guidance
- **Fertilizer library** — track fertilizer type, dosage, and application notes
- **Weather alerts** — publish, filter by severity, and resolve alerts
- **Farmer advisory portal** — searchable crop guides with step-by-step
  application instructions
- Responsive layout with a collapsible sidebar for mobile

## Tech stack

| Layer    | Technology                                  |
|----------|----------------------------------------------|
| Frontend | HTML5, CSS3, vanilla JavaScript, Bootstrap 5 |
| Backend  | PHP 8 (PDO, sessions, CSRF tokens)            |
| Database | MySQL / MariaDB                               |
| Local env| XAMPP (Apache + MySQL)                        |

## Project structure

```
.
├── index.html        # Dashboard markup (login screen + admin layout + modals)
├── styles.css         # All dashboard styling
├── app.js              # Frontend interactivity (demo login, tables, modals, toasts)
├── config.php          # Database connection (PDO)
├── api.php             # Session auth, CSRF, and CRUD endpoints
├── seed_admin.php      # One-time script to create/update the admin account
├── schema.sql          # Database schema + starter seed data
└── SETUP.md            # Local XAMPP setup walkthrough
```

## Getting started

1. Install [XAMPP](https://www.apachefriends.org/) and start **Apache** and
   **MySQL**.
2. Open **phpMyAdmin** and import `schema.sql` to create the `farmadvisor`
   database and its tables.
3. Copy this project into `xampp/htdocs/intern` (or update `DB_*` constants
   in `config.php` to match your own environment).
4. Visit `http://localhost/intern/seed_admin.php` once to create/update the
   admin account.
5. Visit `http://localhost/intern/index.html` and sign in.

See [`SETUP.md`](./SETUP.md) for the detailed walkthrough.

### Configuration

Database credentials live in `config.php` as plain constants for local
development convenience. Before deploying anywhere public, move `DB_HOST`,
`DB_NAME`, `DB_USER`, and `DB_PASS` (and the seeded admin credentials) out of
source control — e.g. into environment variables loaded at runtime — rather
than committing real secrets.

## API overview

`api.php` exposes a small JSON API guarded by session auth + CSRF tokens
(all actions except `csrf`, `login`, and `logout` require an authenticated
admin session):

| Action              | Method | Description                       |
|---------------------|--------|------------------------------------|
| `csrf`               | GET    | Issue/retrieve a CSRF token       |
| `login`              | POST   | Authenticate and start a session  |
| `logout`             | GET/POST | End the current session         |
| `list-alerts`        | GET    | List unresolved weather alerts    |
| `create-alert`       | POST   | Publish a weather alert           |
| `delete-alert`       | POST   | Resolve/remove a weather alert    |
| `create-user`        | POST   | Create a farmer/admin account     |
| `create-crop`        | POST   | Add a crop                        |
| `update-crop`        | POST   | Edit a crop                       |
| `delete-crop`        | POST   | Remove a crop                     |
| `create-fertilizer`  | POST   | Add a fertilizer                  |
| `delete-fertilizer`  | POST   | Remove a fertilizer               |

## Roadmap

- Wire up `app.js` to call `api.php` instead of manipulating the DOM with
  in-memory demo data
- Restore Module 2 features (currently kept aside per `SETUP.md`)
- Add form-level validation and error states for the API responses
- Add automated tests for the API layer

## License

No license has been chosen yet for this project. Add a `LICENSE` file before
sharing it publicly if you want to define usage terms.
