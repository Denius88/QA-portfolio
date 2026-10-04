# Logs and Edge Cases: Smart Monitor Bot

| Field | Value |
|---|---|
| **Application** | Smart Monitor Bot |
| **Author** | Denys Pelekh |
| **Role** | QA Engineer |
| **Environment** | Local macOS, Terminal, Python 3.13, SQLite 3 (`monitor.db`) |
| **Focus** | Background scheduler resilience, timeouts, anti-bot walls, and DOM errors |
| **Execution status** | Executed / Passed |

---

## 1. Overview

Price monitoring relies on recurring background tasks executed by an asynchronous scheduler (APScheduler loop). External e-commerce websites frequently introduce edge cases such as connection drops, Cloudflare challenge walls, and structural HTML updates.

This document details terminal log inspections performed during edge-case test execution, confirming that external failures are handled cleanly without crashing the primary Telegram bot process or stalling queued tasks.

---

## 2. Edge Case Scenarios & Log Inspection

### Edge Case 1: Connection Timeout During Scheduled Request

- **Scenario:** The target store server becomes unresponsive or introduces latency exceeding the 15-second client timeout threshold.
- **Related Test Case:** `TC-SMB-016`
- **Expected Behavior:** `curl_cffi` request timeout is intercepted by `fetch_html`, logged cleanly, and the worker continues running without interrupting other parallel tasks.
- **Terminal Output Observed:**
  ```text
  INFO:smart_monitor.services.scheduler:🔍 Checking: https://store.example/item/lagging-product
  ERROR:smart_monitor.services.scraper:Error fetching https://store.example/item/lagging-product: curl: (28) Timeout was reached after 15000 milliseconds
  INFO:smart_monitor.services.scheduler:Polling cycle completed without uncaught exceptions.
  ```
- **Observation:** The background worker did not crash. The bot interface in Telegram remained fully responsive.
- **Status:** Pass

---

### Edge Case 2: Anti-Bot Challenge (403 Forbidden / Cloudflare Protection)

- **Scenario:** An external marketplace activates bot mitigation (e.g., Cloudflare Turnstile / WAF), returning HTTP status 403 Forbidden with a challenge body instead of product markup.
- **Expected Behavior:** The scraper detects the non-200 status code, logs an explicit error, bypasses parsing, and preserves existing database state.
- **Terminal Output Observed:**
  ```text
  INFO:smart_monitor.services.scheduler:🔍 Checking: https://protected-retailer.example/item/gpu
  ERROR:smart_monitor.services.scraper:Failed to fetch https://protected-retailer.example/item/gpu. Status: 403
  ```
- **Observation:** Handled gracefully; `current_price` remained unchanged in SQLite and no false price drop alerts were triggered.
- **Status:** Pass

---

### Edge Case 3: Store DOM Layout Change (Missing Price Selector)

- **Scenario:** The online store redesigns its product page layout. The HTTP request succeeds (200 OK), but the configured CSS selector for the price element returns None.
- **Expected Behavior:** `extract_price` safely returns `None`, logs the parse mismatch, and the scheduler skips updating the price without crashing.
- **Terminal Output Observed:**
  ```text
  INFO:smart_monitor.services.scheduler:🔍 Checking: https://rozetka.com.ua/item/redesigned
  ERROR:smart_monitor.services.scraper:Error parsing HTML for domain rozetka.com.ua: selector returned None
  ```
- **Observation:** No false alert dispatch; stored price in SQLite was preserved without corrupting historical records.
- **Status:** Pass

---

## 3. Execution Summary

| Scenario | Handled Exception / Status | Impact on Bot Process | Data Integrity Maintained | Status |
|---|---|---|---|:---:|
| **Connection Timeout** | `curl (28) Timeout` | None; scheduler continued loop | Yes | Pass |
| **Anti-Bot Wall** | `HTTP Status 403` | None; logged cleanly | Yes | Pass |
| **DOM Redesign** | Selector mismatch | None; alert suppressed | Yes | Pass |

- **Planned scenarios:** 3
- **Executed:** 3
- **Passed:** 3
- **Failed:** 0
- **Summary:** Terminal log inspection confirms that network timeouts, HTTP 403 responses, and selector mismatches are intercepted gracefully without halting the bot or scheduler.