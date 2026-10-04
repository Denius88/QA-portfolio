# Smoke Test Checklist - Smart Monitor Bot (Critical Flow)

| Field | Value |
|---|---|
| **Application** | Smart Monitor Bot |
| **Author** | Denys Pelekh |
| **Role** | QA Engineer |
| **Execution status** | Executed / Passed |
| **Environment** | Local macOS, Telegram Desktop, SQLite 3 (`monitor.db`) |

## Critical Flow Checks

| ID | Action | Expected Result | Status (Pass/Fail) | Comment / Actual Result |
|---|---|---|---|---|
| CK-SMB-01 | Send `/start` command | Bot initializes user in SQLite, sends welcome greeting, and renders main keyboard (`➕ Add Item`, `📋 My Items`, `📈 Price History`, `🗑 Delete Item`) | Pass | New user record `denispelekh` created in `users` table; persistent keyboard rendered cleanly. |
| CK-SMB-02 | Click `➕ Add Item` (or `/add`) | Bot prompts for product URL (`🔗 Please send me the URL...`) and displays `❌ Cancel` keyboard | Pass | Transitioned into `waiting_for_url` state; cancel button active. |
| CK-SMB-03 | Send valid product URL (Rozetka / Books) | Bot validates `http/https` protocol, accepts URL, and prompts for target price | Pass | URL accepted cleanly; bot prompted for target price. |
| CK-SMB-04 | Send valid target price (e.g., `350`) | Bot accepts numeric price and prompts for polling interval in minutes | Pass | Price parsed as float; prompt for checking interval displayed. |
| CK-SMB-05 | Send valid interval (e.g., `15`) | Bot fetches current price via scraper, saves item to SQLite, and confirms tracking with summary card | Pass | Scraper fetched current price (395 UAH); confirmation card displayed and record inserted into `tracked_items`. |
| CK-SMB-06 | Click `📋 My Items` (or `/list`) | Bot displays list of all tracked items belonging to current user with ID, URL, current price, target, and interval | Pass | Item card displayed with ID 1, Rozetka URL, current price 395 UAH, target 350 UAH, 15 min interval. |
| CK-SMB-07 | Click `📈 Price History` (or `/history`) | Bot returns recent timestamped price snapshots from `price_history` table for tracked products | Pass | Rendered historical price snapshots with UTC timestamps. |
| CK-SMB-08 | Click `🗑 Delete Item` (or `/delete`) and enter valid ID | Bot verifies item ownership, deletes item from `tracked_items`, and confirms deletion | Pass | Ownership verified; item deleted with confirmation message `Item with ID 1 has been deleted.` |
| CK-SMB-09 | Click `❌ Cancel` during input flow | Bot clears active FSM state and confirms: `🚫 Action cancelled. Returning to main menu.` | Pass | State cleared cleanly; returned to main keyboard without saving partial data. |
| CK-SMB-10 | Price drop alert delivery | Background scheduler triggers formatted HTML alert (`🎉 PRICE DROP ALERT!`) when remote price <= target price | Pass | Scheduler detected threshold condition and dispatched formatted Telegram alert with product link. |

## Execution Summary

- **Planned checks:** 10
- **Executed:** 10
- **Passed:** 10
- **Failed:** 0
- **Blocked:** 0
- **Result:** Pass