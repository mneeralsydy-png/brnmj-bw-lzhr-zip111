# Private Dialer - أبو الزهراء

## Overview

Private Dialer is a professional Arabic VoIP application called "أبو الزهراء" (Abu Al-Zahra). It's a web-based phone calling application that allows users to make phone calls through Twilio's API. The app features dual authentication (email/password + Google), a balance/credit system, call history tracking, SMS messaging, and PayPal integration for adding funds. The interface is designed in Arabic (RTL layout) and styled as a mobile-first application resembling a native phone dialer with Capacitor Android support.

## Project Structure

### Frontend (Vanilla HTML/CSS/JS)
- **`client/index.html`** - Main app HTML with full RTL Arabic UI, CSS styles, and inline JS logic
- **`client/public/script.js`** - External JS brain logic (auth, calling, contacts, SMS, payments)
- **`client/public/manifest.json`** - PWA manifest for mobile-app-like experience

### Backend (Node.js + Express + TypeScript)
- **`server/index.ts`** - Express server entry point
- **`server/routes.ts`** - All API routes (register, login, calls, SMS, recordings, topup, etc.)
- **`server/storage.ts`** - Storage interface
- **`database.db`** - SQLite database file (auto-created)

### Mobile (Android / Capacitor)
- **`capacitor.config.json`** - Capacitor configuration for mobile build
- **`android/gradle.properties`** - Android Gradle settings
- **`android/variables.gradle`** - Android SDK version variables
- **`android/app/google-services.json`** - Firebase configuration for Android
- **`android/app/src/main/AndroidManifest.xml`** - Android permissions and activity config

## Architecture

### Backend API Routes (all under `/api/`)
| Route | Method | Auth | Description |
|-------|--------|------|-------------|
| `/api/register` | POST | No | User registration |
| `/api/login` | POST | No | User login (grants $1 on first login) |
| `/api/user` | GET | JWT | Get user info |
| `/api/user-info` | GET | JWT | Get user info + virtual US number |
| `/api/history` | GET | JWT | Call history |
| `/api/call` | POST | JWT | Make outbound call via Twilio |
| `/api/token` | GET | JWT | Get Twilio Voice SDK access token |
| `/api/sms/send` | POST | JWT | Send SMS via Twilio |
| `/api/sms/list` | GET | JWT | List sent SMS messages |
| `/api/recordings` | GET | JWT | Fetch call recordings from Twilio |
| `/api/topup` | POST | JWT | Add balance (after PayPal payment) |
| `/api/firebase-auth` | POST | No | Google/Firebase authentication |
| `/twiml` | POST | No | TwiML webhook for Twilio calls |
| `/token` | GET | No | Legacy Twilio token (for index.html SDK) |
| `/setup-new-user` | POST | No | Firebase new user setup hook |

### Database (SQLite)
- **`users`** - id, email, password (bcrypt), balance, is_new_user
- **`calls`** - id, userId, toNumber, duration, cost, timestamp
- **`sms`** - id, userId, toNumber, message, direction, cost, timestamp

### External Services
- **Firebase** - Authentication (email/password + Google) + Realtime Database
- **Twilio** - VoIP calls + SMS + recordings
- **PayPal** - Payment processing for balance top-up

## Environment Variables Required

| Variable | Purpose |
|----------|---------|
| `TWILIO_ACCOUNT_SID` | Twilio account identifier |
| `TWILIO_AUTH_TOKEN` | Twilio authentication token |
| `TWILIO_PHONE_NUMBER` | Twilio phone number for outbound calls |
| `TWILIO_TWIML_APP_SID` | TwiML app SID for Voice SDK |
| `TWILIO_API_KEY` | (Optional) Twilio API key |
| `TWILIO_API_SECRET` | (Optional) Twilio API secret |
| `JWT_SECRET` | JWT signing secret (defaults to PRIVATE_DIALER_SECRET) |

## User Preferences

Preferred communication style: Simple, everyday language.
