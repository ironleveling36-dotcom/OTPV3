# OTPCart Telegram Bot — v3 (Wallet + Admin Panel + Auto-Retry)

A Telegram OTP-number bot powered by OTPDoctor, now with a full **Wallet System**,
**Admin Control Panel**, **Top Services**, and an **Auto-Retry** engine.

## ✨ Features

### 💳 Wallet System
- Users recharge their wallet via **QR code + UPI** (managed by admin).
- User enters an amount → sees QR/UPI → taps **✅ I Paid**.
- Admin gets an instant notification with **Approve / Reject** buttons.
- On approval, the balance is credited automatically and the user is notified.
- Services are purchased directly from wallet balance — the bot verifies funds
  and deducts the cost before activation.

### 💸 Automatic Refunds
- If a number is **cancelled** before any OTP arrives → amount refunded.
- If **no OTP arrives within 3 minutes** → number auto-cancelled + refunded.
- If a number **expires** with no messages → refunded.
- If all auto-retry attempts fail → refunded.

### 🔁 Auto-Retry System
- On provider errors (`No Number Available`, `Try Again`, etc.), the bot
  automatically retries the **same service every 2 seconds, up to 20 attempts**.
- Stops as soon as a number is received.
- If all 20 attempts fail → service cancelled + wallet refunded.

### ⭐ Top Services
- Admin can mark any service as a **Top Service**.
- Top services appear as **pinned/highlighted 🔥 buttons** at the top of the main menu.

### 🔧 Admin Panel (`/admin`)
- **Services:** add / edit / delete, enable / disable, change prices, mark as Top.
- **User Wallets:** search users, view balances, credit / debit, view tx logs.
- **Transactions:** view full system transaction history.
- **Notifications:** broadcast a message to all users.
- **Payment Details:** update UPI ID and QR-code image at any time.

## 🚀 Setup

### Environment variables
| Variable      | Required | Description                                              |
|---------------|----------|----------------------------------------------------------|
| `BOT_TOKEN`   | ✅       | Telegram bot token from @BotFather.                      |
| `ADMIN_IDS`   | ✅       | Comma-separated Telegram user IDs of admins, e.g. `12345,67890`. |
| `OTP_API_KEY` | optional | OTPDoctor API key (defaults to the bundled key).         |

> Get your numeric Telegram ID from [@userinfobot](https://t.me/userinfobot).

### Install & run
```bash
pip install -r requirements.txt
export BOT_TOKEN="your_token_here"
export ADMIN_IDS="123456789"        # your Telegram ID
python bot.py
```

## 🗂️ Files
| File           | Purpose                                                        |
|----------------|---------------------------------------------------------------|
| `bot.py`       | Main bot: handlers, wallet flow, admin panel, OTP + auto-retry |
| `database.py`  | SQLite persistence (users, wallets, services, tx, settings)   |
| `keyboards.py` | All inline keyboards (user + admin + wallet)                  |
| `otp_api.py`   | OTPDoctor API wrapper                                          |
| `storage.py`   | In-memory active-order tracking                               |
| `config.py`    | Config + timing (3-min OTP timeout)                          |

## 📝 Notes
- Wallet balances, services, transactions and payment settings persist in a local
  `bot.db` SQLite file (created automatically on first run).
- The first time a country's service catalog is opened, services are auto-synced
  from the provider into the local DB so the admin can manage/price them.
