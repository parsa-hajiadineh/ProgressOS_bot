# Bot interface and integrations

The Telegram interface is in Persian. Users enter through `/start` and navigate reply and inline keyboards for subscriptions, payments, wallet withdrawals, referrals, gift cards, and support tickets.

## Commands

| Command | Purpose |
| --- | --- |
| `/start` | Register or open the menu; accepts referral and `gift_CODE` deep links |
| `/report` | Request a personal monthly report |
| `/redeem CODE` | Redeem a boost code; hidden from the menus |
| `/amiadmin` | Display administrator status |
| `/stats`, `/dashboard` | Administrator statistics |
| `/reply TICKET_ID TEXT` | Reply to a support ticket |
| `/msg USER_ID TEXT` | Send a direct administrator message |
| `/broadcast TEXT` | Preview and confirm a broadcast |
| `/msklist` | Message a selected group of users |
| `/createcode CODE PERCENT MAX_USES VALID_UNTIL`, `/listcodes` | Manage discount codes |
| `/createboost CODE L1% L2% MAX_USES VALID_UNTIL`, `/listboosts` | Manage referral boost codes |
| `/makeaffiliate USER_ID MAX_DEPTH RATE%` | Create an affiliate |
| `/updateaffiliate USER_ID FIELD VALUE` | Update affiliate settings |
| `/removeaffiliate USER_ID`, `/listaffiliates` | Deactivate or list affiliates |
| `/reset` | Clear the administrator's own flow state |

Administrator menus also expose purchase and withdrawal approvals, user search, and USDT price configuration. Command argument guidance is available in the bot.

## Integrations

### Telegram

aiogram 2.25.1 uses long polling. The bot needs administrator rights on subscription channels to create invite links and remove members. Membership checks use Telegram's chat membership API.

### Supabase

The Supabase client authenticates with `SUPABASE_URL` and `SUPABASE_KEY`. Tables must be created using [schema.sql](schema.sql), and the configured key must have the required read/write access. See [DATABASE.md](DATABASE.md).

### Nobitex

The pricing helper first checks `usdt_price_irr` in the `config` table. Otherwise it requests `https://api.nobitex.ir/v2/orderbook/USDTIRT`, reads the first ask price, and divides it by ten to convert Rial to Toman. If the request or conversion fails, it falls back to 160,000 Toman.

## HTTP health routes

`GET /` and `GET /health` return status 200 with body `OK`. The server listens on `0.0.0.0` at `PORT` (default `8000`). These routes do not test Telegram or Supabase connectivity.
