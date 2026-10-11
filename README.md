# Telegram Subscription Bot

A Python Telegram bot for managing paid access to private channels. The bot interface is in Persian; records and user flow state are stored in Supabase (PostgreSQL).

## Features

- Normal and Premium subscriptions, a five-minute trial, and subscription expiry reminders.
- USDT (BEP20) and Iranian bank card payments with administrator approval, plus wallet payments.
- Referral commissions, withdrawals, discount codes, boost codes, gift cards, and reservation payments.
- Support tickets and administrator tools for approvals, reports, broadcasts, and user search.

## Setup

Requires Python 3.11, a Telegram bot token, and a Supabase project.

```sh
python -m venv .venv
# Activate the virtual environment for your shell.
python -m pip install -r requirements.txt
```

1. Run [docs/schema.sql](docs/schema.sql) in the Supabase SQL Editor. For an existing database without user flow persistence, run [the state migration](docs/migrations/001_create_user_states.sql).
2. Configure the environment variables listed in [.env.example](.env.example). The application reads the process environment; it does not load `.env` files automatically. Use a server-side Supabase key with permission to read and write the tables.
3. Set administrator IDs, subscription channel IDs, and the payment details needed for your payment methods. Set `BOT_USERNAME` for share links and `SUPPORT_USERNAME` for payment support.
4. Add the bot as an administrator to the configured channels with permission to create invite links and remove members.
5. Start the bot and send `/start` in Telegram:

```sh
python main.py
```

## Deployment

The included Dockerfile runs Python 3.11. `render.yaml` defines a Docker web service; configure its environment variables on Render. Run one polling instance per bot token.

The HTTP server returns `OK` at `/` and `/health` on `PORT` (default `8000`). These endpoints indicate that the HTTP server is running; they do not verify database connectivity.

## Documentation

- [Configuration and project overview](docs/PROJECT.md)
- [Architecture](docs/ARCHITECTURE.md)
- [Bot commands and integrations](docs/API.md)
- [Database reference](docs/DATABASE.md)

## License

[MIT](LICENSE).
