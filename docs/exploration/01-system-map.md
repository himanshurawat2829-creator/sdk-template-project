# System Map — Module 1

## The 5 parts
- frontend (Vanilla JS + Vite) — port 5173
- products-service (PHP/Slim)
- users-service (Python/FastAPI) — port 8000
- orders-service (Java/Spring Boot)
- database (PostgreSQL) — port 5432

## How they connect
All 5 run in Docker containers on one Docker Compose network. Containers reach each
other by service name (e.g. database:5432), not by IP or localhost. The browser
reaches the frontend via localhost:5173 because that port is published to the host.

## One request traced
1. Browser loads http://localhost:5173 (frontend container, Vite dev server)
2. Frontend JS calls fetch() to a backend service over the Docker network
3. That service has a route matching the request path
4. The route queries the database service (PostgreSQL) for data
5. JSON comes back and frontend JS renders it into the page

## Environment gotchas
- Ran docker compose up -d --build without creating a .env file first — every
  variable (DB_PORT, DB_USER, etc.) defaulted to blank and the build failed with
  "invalid proto". Fixed by running cp .env.example .env before building.
- After building, the frontend container threw: "Cannot find module
  '/app/node_modules/vite/dist/node/chunks/dist.js'". This happened because a
  node_modules folder built for macOS existed locally and got out of sync with
  what the Linux container expected. Fixed with:
  docker compose build --no-cache frontend followed by
  docker compose up -d --build --force-recreate frontend
  — the --force-recreate flag was necessary because Docker was reusing a stale
  container instance even after the image was rebuilt.

## One thing the documentation doesn't explain
The README doesn't mention that .env must be created manually from .env.example
before the first docker compose up — without it, docker compose silently defaults
every variable to blank instead of failing immediately with a clear error.
