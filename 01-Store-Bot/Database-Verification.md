# Database Verification: Store Bot

| Field | Value |
|---|---|
| **Application** | Store Bot |
| **Database** | SQLite |
| **Author** | Denis Pelekh |
| **Role** | QA Engineer |
| **Execution status** | Partially verified through automated database tests |

## Database Scope

The database layer contains users, categories, products, cart items, orders, and order items. The checks below focus on persistence, relationships, order totals, cart cleanup, and user data isolation.

## Verification Checklist

### DB-001 - Verify users
2 passed in 0.18s
```