# BUG-SMB-001 - Target price validation accepts negative and zero values

| Field | Value |
|---|---|
| **Bug ID** | BUG-SMB-001 |
| **Title** | Bot accepts zero and negative target prices without validation and saves record into SQLite |
| **Author** | Denys Pelekh |
| **Related test case** | TC-SMB-006 |
| **Severity** | Medium |
| **Priority** | Medium |
| **Status** | Open |
| **Environment** | Local macOS, Telegram Desktop, Python 3.13, SQLite 3 (`monitor.db`) |
| **Component** | Bot FSM / Input Validation (`handlers.py`) |
| **Reproducibility** | Always (100% reproducible) |

## Preconditions

- The Smart Monitor Bot is running locally.
- Active Telegram user account initialized (`/start`).
- Bot is waiting for target price threshold in `waiting_for_price` state.

## Steps to Reproduce

1. Open the bot chat in Telegram Desktop.
2. Click `➕ Add Item` (or send `/add`).
3. Send a valid product URL:
   ```text
   https://books.toscrape.com/catalogue/a-light-in-the-attic_1000/index.html
   ```
4. When prompted: `💰 What is your target price? (e.g., 500 or 1500.50)`, send `-50`.
5. When prompted for interval, send `15`.
6. Inspect the bot confirmation message and query `monitor.db`.

## Expected Result

The bot rejects non-positive values with a validation error message:
```text
❌ Target price must be a positive number greater than 0. Please enter a valid price.
```
The conversation state remains in `waiting_for_price` without advancing or persisting invalid records.

## Actual Result

The bot converts the input via `float()` without boundary validation and advances to interval selection. Upon entering the interval, the bot confirms:
```text
✅ Item saved successfully!

💵 Current price: 51.77
🎯 Target Price: -50.0
⏱ Checking every 15 minutes.
```
A record with `target_price = -50.0` is permanently saved into the `tracked_items` table in SQLite:
```text
id | user_id   | target_price | current_price | check_interval
---+-----------+--------------+---------------+---------------
2  | 719317714 | -50.0        | 51.77         | 15
```

## Impact

- Allows saving invalid business data into the persistence layer.
- Because real-world e-commerce prices cannot be negative, the notification condition `current_price <= target_price` will never evaluate to true.
- Leads to redundant background polling scheduler tasks that waste server resources and external requests indefinitely.

## Suggested Fix

Add a boundary validation check in `smart_monitor/bot/handlers.py` inside `process_price`:

```python
target_price = float(message_text.replace(',', '.'))
if target_price <= 0:
    await message.answer("❌ Target price must be greater than 0. Please enter a positive number.", reply_markup=cancel_kb)
    return
```

## Notes

Found during Boundary Value Analysis (BVA) test execution for `TC-SMB-006`.
