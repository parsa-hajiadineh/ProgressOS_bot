# Architecture

## Runtime

A single Python process runs aiogram long polling and an aiohttp HTTP health server on one asyncio event loop. `main.py` imports the handler modules to register their Telegram message and callback handlers. Shared bot and dispatcher instances live in `bot_instance.py`.

## Components

- `config.py` reads environment variables, validates the token and Supabase settings, initializes the database client, and defines table mappings.
- `main.py` supplies shared subscription, referral, reservation, gift card, pricing, and reporting helpers.
- `handlers/` contains the user and administrator interaction flows; `keyboards.py` builds their keyboards.
- `sheets.py` retains the row-based interface of the former Sheets backend while reading and writing Supabase tables. Returned rows include the internal database ID for updates.
- `jobs.py` handles pending actions, expiry, reminders, trial removal, and monthly reports. Startup rebuilds subscription schedules and starts background jobs.
- `state_store.py` caches user flow state and persists assignments and deletions to the `user_states` table. Startup reloads saved states. If that table is unavailable, the cache remains usable without persistence. Mutating a retrieved dictionary alone does not trigger a database write.

## I/O and storage

Telegram and pricing HTTP requests use asynchronous clients. Supabase calls use the synchronous client inside application helpers, so database requests can block the event loop. The application performs full-table reads for several operations; see [DATABASE.md](DATABASE.md) for the storage interface and schema.

## Deployment

Run one polling instance per bot token. The Dockerfile uses Python 3.11; `render.yaml` defines a Docker web service. Health routes `/` and `/health` return `OK` on the configured `PORT` without checking external services.
