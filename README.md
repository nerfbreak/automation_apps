# Optimize Newspage — Android Client & FastAPI Backend

Native Android client (Kotlin/Jetpack Compose) + FastAPI backend for the Newspage Automation Engine.

## Architecture

- **Android** — Kotlin + Jetpack Compose, Retrofit, WebSocket for real-time logs
- **Backend** — FastAPI, Playwright headless browser, Supabase (PostgreSQL)
- **Deployment** — Railway (Docker), Playwright Chromium

## Project Structure

```
automation_apps/
├── android/          # Kotlin Android app (Jetpack Compose)
│   ├── app/src/main/java/com/newspage/optimize/
│   │   ├── data/     # API service, auth, session, WebSocket
│   │   └── ui/       # Screens, navigation, theme
│   └── build.gradle.kts
├── backend/          # FastAPI backend
│   ├── core/         # Database (Supabase), config
│   ├── routes/       # API endpoints (auth, inventory, sales, promotion, ws)
│   └── services/     # Playwright adapter, job manager, telegram
├── Dockerfile        # Railway deployment
├── railway.toml      # Railway config
└── packages.txt      # System dependencies (Chromium)
```

## Setup

### Backend

```bash
pip install -r backend/requirements.txt
playwright install chromium
uvicorn backend.main:app --host 0.0.0.0 --port 8000
```

### Environment Variables

| Variable | Description |
|----------|-------------|
| `SUPABASE_URL` | Supabase project URL |
| `SUPABASE_KEY` | Supabase service role key |
| `MASTER_KEY` | Fernet encryption key (AES-256) |
| `JWT_SECRET` | JWT signing secret |
| `NP_USER_SUPER` | Newspage superuser (promotion sync) |
| `NP_PASS_SUPER` | Superuser password |
| `TELEGRAM_BOT_TOKEN` | Telegram bot token (alerts) |
| `TELEGRAM_CHAT_ID` | Telegram chat ID |

### Android

1. Create `android/local.properties`:
```properties
sdk.dir=/path/to/android/sdk
API_BASE_URL=https://your-backend-url/
WS_BASE_URL=wss://your-backend-url
```

2. Open `android/` in Android Studio and build.

## API Endpoints

| Method | Path | Description |
|--------|------|-------------|
| POST | `/api/auth/login` | Authenticate user |
| POST | `/api/auth/refresh` | Refresh JWT token |
| GET | `/api/dashboard/stats` | Dashboard statistics |
| GET | `/api/distributors` | List distributors |
| POST | `/api/inventory/extract` | Extract inventory data |
| POST | `/api/inventory/compare` | Compare inventory |
| POST | `/api/inventory/execute` | Execute stock adjustment |
| POST | `/api/sales/extract` | Extract sales data |
| GET | `/api/sales/download/{job_id}` | Download sales CSV |
| POST | `/api/promotion/sync` | Sync promotion data |
| POST | `/api/promotion/compare` | Compare promotions |
| GET | `/api/health` | Health check |
| WS | `/ws/{job_id}?token=` | Real-time job logs |

## License

Built by **Muhammad Rizki Firdaus**
