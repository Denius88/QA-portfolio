# Test Cases: Smart Monitor Bot

| Field | Value |
|---|---|
| **Application** | Smart Monitor Bot |
| **Author** | Denys Pelekh |
| **Role** | QA Engineer |
| **Environment** | Local macOS, Telegram Desktop, SQLite 3 (`monitor.db`) |
| **Execution status** | Executed / Passed |

---

### TC-SMB-001 - New user registration and welcome menu
- **Priority:** High
- **Type:** Functional / Positive
- **Preconditions:** Bot is running; user has not interacted with the bot before.
- **Steps:**
  1. Open the bot chat in Telegram Desktop.
  2. Send `/start`.
- **Expected result:** The bot sends a welcome message (`👋 Welcome to Smart Monitor Bot!`), creates a record in the `users` table with Telegram ID, and renders the main keyboard (`➕ Add Item`, `📋 My Items`, `📈 Price History`, `🗑 Delete Item`).
- **Actual result:** Bot replied with greeting and persistent menu keyboard; user record created in `users` table with Telegram ID `719317714`.
- **Status:** Pass

---

### TC-SMB-002 - Add valid product URL
- **Priority:** High
- **Type:** Functional / Positive
- **Preconditions:** User is on the main menu.
- **Steps:**
  1. Click `➕ Add Item` (or send `/add`).
  2. Verify bot prompts: `🔗 Please send me the URL of the product you want to track:` and displays `❌ Cancel`.
  3. Send a valid URL (e.g., Rozetka product page).
- **Expected result:** The URL is accepted, stored in FSM state, and the bot prompts for target price: `💰 What is your target price? (e.g., 500 or 1500.50)`.
- **Actual result:** URL accepted without errors; FSM transitioned into `waiting_for_price` state with cancel button available.
- **Status:** Pass

---

### TC-SMB-003 - Reject malformed URL format
- **Priority:** High
- **Type:** Negative / Validation
- **Preconditions:** Bot is in `waiting_for_url` state.
- **Steps:**
  1. Send `not-a-link`.
  2. Send `ftp://store.example/item`.
- **Expected result:** Bot rejects both entries with: `❌ Invalid link. Please send a valid URL (http/https).` and remains in the URL input state.
- **Actual result:** Non-HTTP/HTTPS entries rejected with specified error message; bot remained in `waiting_for_url` state.
- **Status:** Pass

---

### TC-SMB-004 - Set valid target price
- **Priority:** High
- **Type:** Functional / Positive
- **Preconditions:** Valid URL accepted; bot is in `waiting_for_price` state.
- **Steps:**
  1. Send `350.00`.
- **Expected result:** Bot accepts the numeric price and prompts for check interval: `⏱ How often should I check the price? (Enter minutes, e.g., 15, 60, 1440)`.
- **Actual result:** Numeric float parsed correctly; bot advanced to interval selection state.
- **Status:** Pass

---

### TC-SMB-005 - Reject non-numeric target price format
- **Priority:** High
- **Type:** Negative / Validation
- **Preconditions:** Bot is in `waiting_for_price` state.
- **Steps:**
  1. Send `abc`.
  2. Send `$50`.
- **Expected result:** Bot rejects the input with: `❌ Invalid format. Please enter a number.` and remains in the price input state.
- **Actual result:** Both invalid string formats rejected with validation error; conversation state preserved.
- **Status:** Pass

---

### TC-SMB-006 - Boundary Value Analysis: Reject zero and negative target prices
- **Priority:** Medium
- **Type:** Boundary / Negative
- **Preconditions:** Bot is in `waiting_for_price` state.
- **Steps:**
  1. Send `0`.
  2. Send `-10.50`.
- **Expected result:** Bot rejects zero and negative values with a validation error message (`Target price must be greater than 0`) and prompts for re-entry.
- **Actual result:** Bot accepted zero and negative values, advancing the FSM and saving the record to SQLite without validation error. Defect logged as [BUG-SMB-001](Bug-Reports/BUG-SMB-001.md).
- **Status:** Failed

---

### TC-SMB-007 - Set valid check interval and save item
- **Priority:** High
- **Type:** Functional / Positive
- **Preconditions:** Bot is in `waiting_for_interval` state.
- **Steps:**
  1. Send `15`.
- **Expected result:** Bot initiates scraper check (`⏳ Checking the current price...`), saves record into `tracked_items`, inserts initial row into `price_history`, and displays confirmation card with current price, target price, and interval.
- **Actual result:** Scraper retrieved current price (395 UAH); item saved in SQLite and confirmation card rendered with summary metrics.
- **Status:** Pass

---

### TC-SMB-008 - Boundary Value Analysis: Reject interval below 1 minute
- **Priority:** Medium
- **Type:** Boundary / Negative
- **Preconditions:** Bot is in `waiting_for_interval` state.
- **Steps:**
  1. Send `0`.
  2. Send `-5`.
  3. Send `fifteen`.
- **Expected result:** For all invalid inputs, bot returns: `❌ Please enter a valid number of minutes (e.g., 15).` and preserves state.
- **Actual result:** All non-positive or non-digit interval inputs rejected cleanly with validation prompt.
- **Status:** Pass

---

### TC-SMB-009 - View tracked items list
- **Priority:** High
- **Type:** Functional / Positive
- **Preconditions:** User has active items saved.
- **Steps:**
  1. Click `📋 My Items` (or send `/list`).
- **Expected result:** Bot returns formatted card list containing `🆔 ID`, `🔗 URL`, `💵 Current`, `🎯 Target`, and `⏱ Checks every: X mins`.
- **Actual result:** Formatted item card rendered displaying item ID 1, product URL, current price 395.0, target 350.0, and interval 15 mins.
- **Status:** Pass

---

### TC-SMB-010 - View price history
- **Priority:** Medium
- **Type:** Functional / Positive
- **Preconditions:** User has tracked items with recorded price history.
- **Steps:**
  1. Click `📈 Price History` (or send `/history`).
- **Expected result:** Bot renders recent 5 historical price snapshots per item formatted with price and UTC timestamp (`💵 XX.XX — YYYY-MM-DD HH:MM UTC`).
- **Actual result:** Price history list displayed with chronological price snapshots and UTC timestamps.
- **Status:** Pass

---

### TC-SMB-011 - Delete tracked item with valid ID
- **Priority:** High
- **Type:** Functional / Positive
- **Preconditions:** User has active tracked item (e.g., ID 1).
- **Steps:**
  1. Click `🗑 Delete Item` (or send `/delete`).
  2. Verify prompt: `🗑 Please send me the **ID** of the item you want to delete:`.
  3. Send valid item ID `1`.
- **Expected result:** Bot removes item from database and confirms: `✅ Item with ID 1 has been deleted.`.
- **Actual result:** Record deleted from `tracked_items`; confirmation message displayed and user returned to main menu.
- **Status:** Pass

---

### TC-SMB-012 - Reject non-numeric delete ID
- **Priority:** Medium
- **Type:** Negative / Validation
- **Preconditions:** Bot is in `waiting_for_id` state.
- **Steps:**
  1. Send `abc`.
- **Expected result:** Bot rejects with: `❌ Invalid ID. Please send a number.`.
- **Actual result:** Validation error displayed; state preserved until numeric ID or cancel was sent.
- **Status:** Pass

---

### TC-SMB-013 - Enforce user data isolation on deletion
- **Priority:** High
- **Type:** Security / Data Isolation
- **Preconditions:** User B owns item ID 999; User A interacts with bot.
- **Steps:**
  1. As User A, send `/delete`.
  2. Enter item ID `999`.
- **Expected result:** Bot rejects deletion with: `❌ Item not found or it doesn't belong to you.` without altering database.
- **Actual result:** Deletion refused with `Item not found or it doesn't belong to you.`; database record intact.
- **Status:** Pass

---

### TC-SMB-014 - Cancel active conversation flow
- **Priority:** Low
- **Type:** Functional / Navigation
- **Preconditions:** User is in any multi-step FSM state (`waiting_for_url`, `waiting_for_price`, etc.).
- **Steps:**
  1. Click `❌ Cancel` (or send `/cancel`).
- **Expected result:** Bot clears active FSM state and confirms: `🚫 Action cancelled. Returning to main menu.`.
- **Actual result:** Conversation state cleared; confirmation sent and main keyboard restored.
- **Status:** Pass

---

### TC-SMB-015 - Price drop alert trigger
- **Priority:** High
- **Type:** Integration / Notifications
- **Preconditions:** User tracks an item where target price is `>=` new fetched price.
- **Steps:**
  1. Background scheduler executes `check_all_prices`.
  2. Scraper detects price change where `current_price <= target_price`.
- **Expected result:** Bot dispatches Telegram message: `🎉 PRICE DROP ALERT! 🎉` containing the new price, target price, and product link.
- **Actual result:** Alert message received in chat with formatted HTML price comparison and direct product URL.
- **Status:** Pass

---

### TC-SMB-016 - Scraper timeout resilience
- **Priority:** High
- **Type:** Negative / Resilience
- **Preconditions:** Product URL server has high latency (> 15 seconds) or is unreachable.
- **Steps:**
  1. Scraper attempts to fetch URL with 15s timeout.
- **Expected result:** Timeout is caught cleanly in `scraper.py`, error is logged (`Error fetching URL: ...`), and the scheduler process continues running without crashing.
- **Actual result:** Exception caught cleanly; logged in terminal; background scheduler remained operational.
- **Status:** Pass

---

## Execution Summary

- **Total test cases:** 16
- **Executed:** 16
- **Passed:** 15
- **Failed:** 1 (`TC-SMB-006`)
- **Blocked:** 0
- **Defects logged:** 1 ([BUG-SMB-001](Bug-Reports/BUG-SMB-001.md))
- **Execution result:** 15 passed, 1 failed due to missing boundary validation on negative target prices.