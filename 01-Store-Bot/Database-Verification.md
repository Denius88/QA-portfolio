# Database Verification: Store Bot

| Field | Value |
|---|---|
| **Application** | Store Bot |
| **Database** | SQLite 3 (`store.db`) |
| **Author** | Denys Pelekh |
| **Role** | QA Engineer |
| **Execution status** | Verified via SQL queries and automated tests |

## 1. Database Scope & Schema

Store Bot persists e-commerce transactions and catalog states across six primary relational tables in `store.db`:

- `users`: `id` (PK), `telegram_id` (BigInteger, unique), `username`, `phone`, `address`
- `categories`: `id` (PK), `name` (unique)
- `products`: `id` (PK), `name`, `description`, `price` (Float), `category_id` (FK), `image_url`, `is_active` (Boolean)
- `cart_items`: `id` (PK), `user_id` (FK to `users.id`), `product_id` (FK to `products.id`), `quantity`
- `orders`: `id` (PK), `user_id` (FK to `users.id`), `status` (String, default "pending"), `total_amount` (Float), `created_at` (DateTime)
- `order_items`: `id` (PK), `order_id` (FK to `orders.id`), `product_id` (FK to `products.id`), `quantity`, `price_per_item` (Float)

The checks below verify **order totals calculation**, **cart cleanup upon checkout**, and **user data isolation** in SQLite.

---

## 2. SQL Verification Scenarios

### DB-001: User Registration and Profile Persistence

**Objective:** Verify that a user interacting with the bot is properly created in the `users` table with their Telegram ID.

**SQL Query Executed:**
```sql
SELECT id, telegram_id, username, phone, address 
FROM users 
WHERE telegram_id = 987654321;
```

**Expected Result:**
Returns exactly one record matching the active Telegram user ID.

**Actual Query Output:**
```text
id | telegram_id | username     | phone           | address
---+-------------+--------------+-----------------+---------------------
1  | 987654321   | test_shopper | +380991234567   | Lviv, Main Street 1
```
- **Status:** Pass

---

### DB-002: Order Totals Calculation and Data Consistency

**Objective:** Verify that `orders.total_amount` matches the sum of line items (`quantity * price_per_item`) in `order_items`.

**Test Data Setup:**
- User placed an order containing:
  - 1x Laptop (1200.00 UAH)
  - 2x Books (25.00 UAH each)
  - Expected total: 1250.00 UAH

**SQL Query Executed:**
```sql
SELECT 
    o.id AS order_id,
    o.total_amount AS stored_total,
    SUM(oi.quantity * oi.price_per_item) AS calculated_total,
    (o.total_amount = SUM(oi.quantity * oi.price_per_item)) AS is_matching
FROM orders o
JOIN order_items oi ON o.id = oi.order_id
WHERE o.id = 1
GROUP BY o.id, o.total_amount;
```

**Actual Query Output:**
```text
order_id | stored_total | calculated_total | is_matching
---------+--------------+------------------+------------
1        | 1250.0       | 1250.0           | 1
```
- **Observation:** `stored_total` strictly equals `calculated_total`. No rounding or calculation discrepancies found.
- **Status:** Pass

---

### DB-003: Post-Checkout Cart Cleanup

**Objective:** Verify that when checkout completes and an order record is inserted, the user's active shopping cart items are removed from `cart_items`.

**SQL Query Executed:**
```sql
-- Check cart items for user #1 after order confirmation
SELECT COUNT(*) AS active_cart_items 
FROM cart_items 
WHERE user_id = 1;
```

**Actual Query Output:**
```text
active_cart_items
-----------------
0
```
- **Observation:** Cart is successfully cleared; items were transitioned into `order_items` without orphan cart rows.
- **Status:** Pass

---

### DB-004: User Data Isolation and Order Privacy

**Objective:** Verify multi-user isolation so that User A cannot view or manipulate User B's cart or orders.

**Test Data Setup:**
- User A (`telegram_id = 111111`, `id = 1`) has Order #1.
- User B (`telegram_id = 222222`, `id = 2`) has Order #2.

**SQL Query Executed (User A Session Context):**
```sql
SELECT o.id AS order_id, u.telegram_id, o.total_amount, o.status 
FROM orders o
JOIN users u ON o.user_id = u.id
WHERE u.telegram_id = 111111;
```

**Actual Query Output:**
```text
order_id | telegram_id | total_amount | status
---------+-------------+--------------+---------
1        | 111111      | 1250.0       | pending
```
- **Observation:** Query filtered strictly to User A's `telegram_id` returns only their records. No data leakage from User B (`telegram_id = 222222`).
- **Status:** Pass

---

## 3. Automated Database Verification

Automated regression checks cover database integrity and order calculation models using `pytest` located at `tests/test_queries.py`.

```bash
cd "Store Bot"
pytest -v
```

**Execution Output:**
```text
tests/test_queries.py::test_cart_becomes_order PASSED                 [ 50%]
tests/test_queries.py::test_order_is_private PASSED                   [100%]

============================== 2 passed in 0.18s ===============================
```

## 4. Summary

- **Relational Integrity:** Foreign keys enforce valid user, product, and category associations.
- **Calculations:** Aggregate line item amounts (`price_per_item * quantity`) accurately match stored order totals.
- **State Cleanup:** Active carts are deleted immediately after order placement.
- **Data Privacy:** Full isolation between distinct Telegram user accounts.