# Wash World

A small web app for a car wash company. Users can sign up, log in, see washing locations on a map, pick a membership, mark favorite locations, start a wash and see their wash history.

Everything runs in Docker, so you only need Docker installed.

## What's inside

| Part | Technology | Address |
|------|------------|---------|
| Frontend (the website) | Next.js / React | http://localhost:3000 |
| Backend (the API) | Python Flask | http://localhost:80 |
| Database | MariaDB | `localhost:3306` (this machine only) |
| Database viewer | phpMyAdmin | http://localhost:8080 (this machine only) |

Project folders:

- `washworld_eksame_frontend_2026/` – the website
- `washworld_eksamen_backend_2026/` – the Flask API
- `db/init/01-schema.sql` – creates the database tables the first time the database starts

## Getting started

1. Install [Docker Desktop](https://www.docker.com/products/docker-desktop/).
2. Copy the example settings file:

   ```powershell
   Copy-Item .env.example .env
   ```

3. Start everything:

   ```powershell
   docker compose up --build
   ```

4. Open http://localhost:3000 in your browser.

To stop the app press `Ctrl+C`, or run `docker compose down`.

## Settings (`.env`)

| Variable | What it does |
|----------|--------------|
| `DB_ROOT_PASSWORD` | Database password. Must match the password in `washworld_eksamen_backend_2026/x.py`. |
| `NEXT_PUBLIC_API_URL` | Address the browser uses to reach the API. Default: `http://localhost:80`. |
| `NGROK_AUTHTOKEN` | Only needed if you use ngrok (see below). |

If you change `NEXT_PUBLIC_API_URL`, rebuild with `docker compose up --build`.

## Ways to run it

**Normal run** (`docker compose up`): the backend reloads automatically when you edit its code.

**Development with hot reload for both parts** (frontend and backend):

```powershell
docker compose -f compose.dev.yml up --build
```

**Production-like run** (no debug mode, no database port, phpMyAdmin off):

```powershell
docker compose -f docker-compose.yml -f docker-compose.prod.yml up -d --build
```

Add `--profile tools` to also start phpMyAdmin.

**Share it publicly with ngrok** (for demos): put your token in `.env`, then:

```powershell
docker compose -f docker-compose.yml -f docker-compose.ngrok.yml up -d ngrok
```

Open http://localhost:4040 to see the public URLs. Set `NEXT_PUBLIC_API_URL` to the public API URL and rebuild.

## Useful commands

```powershell
docker compose logs -f web      # watch backend logs
docker compose ps               # see what is running
docker compose down             # stop everything
docker compose down -v          # stop and DELETE the database data
```

## Good to know

- The database tables are only created the first time. To start fresh, run `docker compose down -v` and start again.
- Port 80 and 3000 must be free on your computer.

## Main API endpoints

- Account: `/api-signup`, `/api-login`, `/forgot-password`, `/reset-password`, `/api-delete-account`
- Profile: `/api-my-info`, `/api-update-my-info`
- Memberships: `/api-memberships`, `/api-my-membership`, `/api-update-my-membership`
- Washing: `/api-start-wash`, `/api-my-wash-history`
- Favorites: `/api-favorites`
