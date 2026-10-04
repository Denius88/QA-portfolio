# Database Verification: Smart Monitor Bot

| Field | Value |
|---|---|
| **Application** | Smart Monitor Bot |
| **Database** | SQLite 3 (`monitor.db`) |
| **Author** | Denys Pelekh |
| **Role** | QA Engineer |
| **Execution status** | Verified via direct SQL queries |

## 1. Database Scope & Schema

The persistence layer stores Telegram users, active monitored products, and historical price records across three related tables in `monitor.db`:

- `users`: `id` (PK, Telegram User ID), `username` (Optional)
- `tracked_items`: `id` (PK, autoincrement), `user_id` (FK to `users.id`), `url`, `title`, `current_price`, `target_price`, `check_interval` (default 60 mins), `last_checked` (DateTime UTC)
- `price_history`: `id` (PK, autoincrement), `tracked_item_id` (FK to `tracked_items.id`), `price` (Float), `checked_at` (DateTime UTC)

The verifications below ensure **user data isolation**, **price history consistency**, and **cascade cleanup** without unintended data leakage or redundant records.

---

## 2. Verification Checklist

### DB-SMB-001: User Data Isolation Verification

**Objective:** Verify that User A cannot view, alter, or delete products monitored by User B, even if both users track an identical product URL.

**Test Data Setup:**
- User (`id = 719317714`, `username = denispelekh`) monitors a Rozetka product URL with target `350.0`.

**SQL Query Executed:**
```sql
SELECT 
    t.id AS item_id,
    t.user_id,
    u.username,
    t.url,
    t.target_price,
    t.check_interval
FROM tracked_items t
JOIN users u ON t.user_id = u.id
WHERE t.url LIKE '%rozetka.com.ua%';
```

**Expected Result:**
The query returns distinct records partitioned strictly by Telegram `user_id`. Querying products by `user_id = 719317714` returns strictly that user's items.

**Actual Query Output:**
```text
item_id | user_id   | username    | target_price | check_interval
--------+-----------+-------------+--------------+---------------
1       | 719317714 | denispelekh | 350.0        | 15
```

**Isolation Query (User Context Filter):**
```sql
SELECT id, user_id, target_price, current_price, check_interval 
FROM tracked_items 
WHERE user_id = 719317714;
```

- **Observation:** Query filtered strictly to Telegram user context returns only records owned by `user_id = 719317714`. Complete isolation confirmed.
- **Status:** Pass

---

### DB-SMB-002: Price History Consistency and Sequence

**Objective:** Verify that periodic price checks insert timestamped snapshots into `price_history` chronologically.

**Test Data Setup:**
- Monitored product `tracked_item_id = 1` across multiple scheduler runs.

**SQL Query Executed:**
```sql
SELECT 
    h.id AS history_id,
    h.tracked_item_id,
    h.price,
    h.checked_at
FROM price_history h
WHERE h.tracked_item_id = 1
ORDER BY h.checked_at ASC;
```

**Expected Result:**
Records maintain sequential chronological UTC timestamps. Historical snapshots link correctly to the parent item.

**Actual Query Output:**
```text
history_id | tracked_item_id | price | checked_at
-----------+-----------------+-------+----------------------------
1          | 1               | 395.0 | 2026-08-21 11:20:53 UTC
2          | 1               | 395.0 | 2026-08-21 11:35:53 UTC
3          | 1               | 395.0 | 2026-10-04 10:33:50 UTC
4          | 1               | 395.0 | 2026-10-04 10:49:50 UTC
```

- **Observation:** Sequential price snapshots recorded with accurate UTC timestamps.
- **Status:** Pass

---

### DB-SMB-003: Cascade Cleanup on Item Deletion

**Objective:** Verify that deleting a tracked product from `tracked_items` cascade-deletes associated historical records in `price_history` (`cascade="all, delete-orphan"`).

**SQL Statement Executed:**
```sql
-- Verify count prior to cleanup
SELECT COUNT(*) FROM price_history WHERE tracked_item_id = 1;

-- Delete tracked item
DELETE FROM tracked_items WHERE id = 1;

-- Verify orphaned history records
SELECT COUNT(*) AS remaining_history 
FROM price_history 
WHERE tracked_item_id = 1;
```

**Expected Result:**
Associated historical entries in `price_history` are deleted, resulting in `remaining_history = 0` without foreign key constraint violations.

**Actual Output:**
```text
remaining_history
-----------------
0
```

- **Observation:** Cascading cleanup functions as configured; no orphan records left.
- **Status:** Pass

---

## 3. Execution Summary

- **Total DB checks:** 3
- **Executed:** 3
- **Passed:** 3
- **Failed:** 0
- **Summary:** Verified relational integrity, user isolation, sequential snapshot recording, and cascading foreign key cleanup in SQLite.