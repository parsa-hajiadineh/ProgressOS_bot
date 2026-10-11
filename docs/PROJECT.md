# Project overview

The bot sells and manages access to private Telegram channels through a Persian interface. It runs as one Python 3.11 process using aiogram 2.25.1 long polling, aiohttp for HTTP requests and health checks, and supabase-py for PostgreSQL storage.

## Configuration

The application reads process environment variables at startup. No dotenv loader is included. `.env.example` lists all supported variables without credentials.

| Variable | Default | Purpose |
| --- | --- | --- |
| `BOT_TOKEN` | Required | Telegram bot token |
| `SUPABASE_URL` | Required | Supabase project URL |
| `SUPABASE_KEY` | Required | Server-side API key with table read/write access |
| `ADMIN_TELEGRAM_ID` | Unset | Primary administrator Telegram user ID |
| `ADMIN2_TELEGRAM_ID` | Unset | Secondary administrator Telegram user ID |
| `REQUIRED_CHANNELS` | Empty | Comma-separated channels for membership checks |
| `NORMAL_CHANNEL_ID` | Unset | Normal subscription channel |
| `PREMIUM_CHANNEL_ID` | Unset | Premium subscription channel |
| `TEST_CHANNEL_ID` | Unset | Trial channel |
| `NORMAL_PRICE` | `5` | Normal subscription price in USD |
| `PREMIUM_PRICE` | `20` | Premium subscription price in USD |
| `TETHER_WALLET` | Empty | Receiving USDT BEP20 address |
| `CARD_NUMBER` | Empty | Receiving Iranian bank card number |
| `CARD_HOLDER` | Empty | Receiving card holder name |
| `BOT_USERNAME` | `YourBot` | Bot username for share links |
| `SUPPORT_USERNAME` | `@YourSupportAccount` | Payment support account |
| `PORT` | `8000` | HTTP health server port |

Subscription and payment settings must be configured for the flows you intend to use. Keep tokens and database keys outside version control.

## Subscription and payment flows

Normal and Premium subscriptions last 180 days. The trial lasts five minutes. USDT BEP20 and bank card payments require administrator approval of the submitted payment evidence. Referral wallet balances and gift cards provide additional purchase flows. Reservation payments begin with a $2 deposit and are completed separately.

The bank card flow uses a configured USDT price or a Nobitex rate, with a fallback of 160,000 Toman. Despite the `irr` naming in code and database fields, this conversion uses Toman.

## Repository layout

| Path | Responsibility |
| --- | --- |
| `main.py` | Entry point, shared business logic, and HTTP health server |
| `config.py` | Environment configuration and table definitions |
| `bot_instance.py` | Shared bot and dispatcher instances |
| `handlers/` | Registration, subscription, wallet, support, and admin handlers |
| `keyboards.py` | Telegram keyboard builders |
| `jobs.py` | Background processing and subscription schedules |
| `sheets.py` | Supabase access through the row-based compatibility interface |
| `state_store.py` | Cached user flow state with Supabase persistence |
| `docs/` | Public documentation, schema, and migration |
| `Dockerfile`, `render.yaml` | Container and Render deployment configuration |
