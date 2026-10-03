# Regression Checklist: Store Bot

| Field | Value |
|---|---|
| **Application** | Store Bot |
| **Author** | Denis Pelekh |
| **Role** | QA Engineer |
| **Environment** | Local, macOS, Telegram |
| **Execution status** | Completed |

## Critical Regression Flow

| ID | Area | Check | Expected result | Status | Notes |
|---|---|---|---|---|---|
| REG-001 | Startup | Start the bot and send `/start` | The bot starts and displays the main menu. | Pass | Verified during regression execution. |
| REG-002 | Catalog | Open the catalog and select a category | Categories and active products are displayed. | Pass | Verified during regression execution. |
| REG-003 | Product details | Open a product card | Name, description, price, photo, and actions are displayed. | Pass | Verified during regression execution. |
| REG-004 | Cart | Add a product to the cart | The product is added with quantity 1 and the correct total. | Pass | Verified during regression execution. |
| REG-005 | Cart quantity | Increase and decrease quantity | Quantity and totals are recalculated correctly. | Pass | Verified during regression execution. |
| REG-006 | Cart removal | Remove the product and clear the cart | The product is removed and the empty-cart state is displayed. | Pass | Verified during regression execution. |
| REG-007 | Checkout | Create an order with valid data | The order is created and the cart is cleared. | Pass | Verified during regression execution. |
| REG-008 | Order history | Open the user's order history and order details | The correct status, items, quantities, and total are displayed. | Pass | Verified during regression execution. |
| REG-009 | Access control | Open `/admin` as a regular user | Access is denied and admin functions are not exposed. | Pass | Verified during regression execution. |
| REG-010 | Admin catalog | Create or edit a product as administrator | Changes are saved and visible in the customer catalog. | Pass | Verified during regression execution. |
| REG-011 | Order status | Change an order status as administrator | The status changes and the customer receives a notification. | Pass | Verified during regression execution. |
| REG-012 | Backup | Run `/backup` as administrator | A database backup is created successfully. | Pass | Verified during regression execution. |

## Execution Summary

- **Planned checks:** 12
- **Executed:** 12
- **Passed:** 12
- **Failed:** 0
- **Blocked:** 0
- **Test date:** 03.10.2026

Regression testing should be executed after catalog, cart, checkout, admin, or database changes.
