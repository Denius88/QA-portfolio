# Test Plan: Smart Monitor Bot (Price Monitoring Application)

| Field | Value |
|---|---|
| **Author** | Denys Pelekh |
| **Role** | QA Engineer |
| **Document version** | 1.0.0 |
| **Status** | Completed |
| **Application** | Smart Monitor Bot (Telegram Application) |
| **Target build** | v1.0-beta |
| **Test date** | 04.10.2026 |
| **Environment** | Local, macOS, Telegram Desktop, SQLite |

## 1. Introduction and Objectives

Smart Monitor Bot is a Telegram application that allows users to track product prices on supported e-commerce platforms. The application accepts product URLs, schedules background polling intervals, checks target price thresholds, and sends notifications when prices drop below the configured values.

The purpose of this test plan is to define the testing strategy, scope, execution steps, and verification methods to ensure stable bot command navigation, accurate price tracking, database isolation, resilient scraper error handling, and scheduler stability.

### Testing Objectives

- Verify bot command handling (`/start`, `/help`, `/add_item`, `/items`, `/delete`, `/interval`).
- Validate input parsing for product URLs and user-defined price thresholds.
- Verify polling intervals and background scheduler execution.
- Validate data persistence and strict user data isolation in SQLite.
- Verify scraper behavior during web edge cases: `404 Not Found`, anti-bot challenges (`403 Forbidden`), and DOM layout changes.
- Verify background worker resilience: verify that connection timeouts and parser exceptions are caught, logged, and do not crash the bot process.

## 2. Scope of Testing

### 2.1 In Scope

- **Command and Conversational Flow:** Main menu, interactive reply/inline buttons, multi-step FSM (Finite State Machine) inputs.
- **Input Validation:** Supported e-commerce URLs, invalid/malformed links, positive prices, zero/negative price thresholds, fractional values.
- **Background Scheduler:** Scheduled polling routines, task queuing, execution intervals (e.g., 15 min, 60 min, 24 h).
- **Price Alert Notifications:** Telegram alert delivery upon reaching or falling below the threshold.
- **Data Integrity and Isolation:** SQLite persistence, relational links between users, monitored items, and price history snapshots.
- **Scraper Fault Tolerance:** Handling HTTP client errors, connection timeouts, unavailable product pages, and dynamic page layout changes.
- **Log Inspection:** Monitoring terminal/application logs for uncaught runtime exceptions and scheduler failures.

### 2.2 Out of Scope

- Load testing with thousands of concurrent scrapers.
- Bypassing commercial CAPTCHA providers (e.g., Cloudflare Turnstile, reCAPTCHA Enterprise).
- Reverse engineering unsupported external marketplace private APIs.
- Payment gateway integrations (free-tier bot scope).

## 3. Test Strategy and Test Types

| Test Type | Focus Area | Technique / Tool |
|---|---|---|
| **Functional Testing** | Command navigation, item tracking, notification triggers | Telegram Desktop, Manual checks |
| **Negative Testing** | Invalid URLs, unparseable input, corrupted payloads | Manual boundary input |
| **Boundary Value Analysis (BVA)** | Minimum price thresholds ($0.01, 0, -1) and polling interval boundaries | Equivalence and boundary partitioning |
| **Database Verification** | User data isolation, price history consistency, foreign keys | SQLite CLI, SQL queries |
| **Resilience & Edge-Case Testing** | `404`, `403` anti-bot walls, connection timeouts, selector mismatch | Mock server, live target testing |
| **Log Inspection** | Background task health, exception traces, scheduler loop stability | macOS Terminal, application stdout/stderr |

## 4. Test Environment and Test Data

### 4.1 Environment

- **Operating System:** macOS (Local development environment)
- **Application Framework:** Python 3.12, aiogram 3.x
- **Client Application:** Telegram Desktop
- **Database:** SQLite 3
- **Background Scheduler:** APScheduler / Asyncio background loop
- **HTTP/Scraping Engine:** httpx, BeautifulSoup4

### 4.2 Test Data

- **Supported Valid URL:** Public product page from a supported test store.
- **Unsupported Platform URL:** `https://en.wikipedia.org/wiki/E-commerce`
- **Malformed URL:** `http://invalid-url-pattern`
- **Missing / 404 URL:** Valid store domain pointing to a deleted or non-existent product route.
- **Anti-bot Simulation:** Mock endpoint or URL returning HTTP status `403 Forbidden`.
- **Target Price Values:** Valid integer (`500`), valid decimal (`49.99`), boundary zero (`0`), invalid negative (`-10`), non-numeric text (`abc`).
- **Interval Values:** Valid (`15m`, `60m`, `24h`), invalid/restricted boundary (`0m`, `2m`, `-5m`).

## 5. Entry Criteria

Testing can begin when:

- The Telegram bot process starts cleanly and connects via the Telegram Bot API.
- The SQLite database schema initializes tables (`users`, `items`, `price_history`).
- Network access to supported test e-commerce storefronts is active.
- Terminal logging is configured to output `INFO` and `ERROR` levels.

## 6. Exit Criteria

Testing is considered complete when:

- All critical functional and smoke checks pass.
- BVA boundaries for thresholds and intervals are enforced by validation rules.
- Negative tests for web scraping (404, anti-bot, layout changes) do not terminate the scheduler loop.
- SQLite checks confirm complete data isolation between separate Telegram `user_id` records.
- All observed defects are documented with actionable logs and repro steps.

## 7. Deliverables

- `Test-Plan.md`
- `Checklist.md` (Smoke Checklist)
- `Test-Design-Techniques.md` (BVA, EP, State Transitions, Decision Tables)
- `Test-Cases.md`
- `Database-Verification.md` (SQL queries and validation logs)
- `Logs-and-Edge-Cases.md` (Scheduler timeouts and error inspection)
- `Test-Summary-Report.md`