# Test Design Techniques: Smart Monitor Bot

| Field | Value |
|---|---|
| **Application** | Smart Monitor Bot |
| **Author** | Denys Pelekh |
| **Role** | QA Engineer |
| **Focus** | Boundary Value Analysis, Equivalence Partitioning, State Transition, Decision Tables |

This document details the practical application of core test design techniques to the Smart Monitor Bot test scenarios.

## 1. Boundary Value Analysis (BVA)

Boundary Value Analysis was applied to numeric boundaries, including target price thresholds and background polling intervals, to evaluate application robustness and identify validation gaps.

### Target Price Threshold

The target threshold determines when an alert should trigger. Values are expected to be positive numbers.

| Field / Parameter | Boundary Value | Test Type | Expected Behavior vs Actual Observation |
|---|---|---|---|
| Target Price | `0.00` | Boundary | Zero threshold: In specification, prices should be positive. Verified if parsed without crashing. |
| Target Price | `-10.00` | Invalid Boundary | Negative values: Evaluates whether input parser enforces strictly positive numbers. |
| Target Price | `0.01` | Valid Boundary | Smallest positive fractional value accepted. |
| Target Price | `999999.00` | Valid Upper Boundary | Large realistic price value accepted. |

Related test cases: `TC-SMB-004`, `TC-SMB-005`, `TC-SMB-006`.

### Polling Intervals

Polling intervals control the frequency of background scraper jobs in minutes.

| Parameter | Boundary Value | Test Type | Expected Behavior |
|---|---|---|---|
| Interval (Minutes) | `0m`, `-5m` | Invalid Boundary | Values below 1 minute are rejected with validation prompt. |
| Interval (Minutes) | `1m` | Minimum Valid Boundary | The minimum acceptable interval in code (1 minute) is accepted. |
| Interval (Minutes) | `15m`, `60m` | Typical Valid | Standard operational intervals accepted. |
| Interval (Minutes) | `1440m` (24h) | Upper Boundary | 24-hour interval accepted. |

Related test cases: `TC-SMB-007`, `TC-SMB-008`.

---

## 2. Equivalence Partitioning (EP)

Input data was partitioned into valid and invalid classes to optimize test coverage.

### Product URL Input

| Input Category | Valid Partitions | Invalid Partitions |
|---|---|---|
| **URL Protocol & Format** | URLs starting with `https://` or `http://` on supported scrapers (`rozetka.com.ua`, `epicentrk.ua`, `amazon`, `ebay`, `books.toscrape.com`) | Non-HTTP protocol (`ftp://...`), plain text strings (`not-a-link`), empty strings |

### Numeric Input Parsing

| Input Category | Valid Partitions | Invalid Partitions |
|---|---|---|
| **Price Values** | Integer values (`100`), standard decimal floats with dot or comma (`99.99`, `99,99`) | Alphabetic strings (`abc`), special characters (`$100`, `@#%`), empty strings |
| **Delete ID** | Numeric IDs belonging to the active user (`1`, `2`) | Non-numeric strings (`abc`), IDs belonging to another user, negative numbers |

---

## 3. State Transition: Monitored Item Lifecycle

Monitored products transition through distinct operational states based on user interaction and scheduler checks.

```text
[ Main Menu ]
       |
       | User clicks '➕ Add Item' (or /add)
       v
[ State: waiting_for_url ]
       |
       | Valid URL provided
       v
[ State: waiting_for_price ]
       |
       | Numeric price provided
       v
[ State: waiting_for_interval ]
       |
       | Interval >= 1 provided
       v
[ Item Created & Active in SQLite ] <-------------------------+
       |                                                      |
       | Scheduler check: remote price > target price         |
       +------------------------------------------------------+
       |
       | Scheduler check: remote price <= target price (price changed)
       v
[ Alert Triggered (Telegram Notification Sent) ]
       |
       | Next cycle: remote price remains same <= target price
       v
[ Alert Suppressed (No Duplicate Alert Sent) ]
       |
       | User clicks '🗑 Delete Item' -> enters Item ID
       v
[ Deleted from SQLite (Cascade deletes price_history) ]
```

---

## 4. Decision Table: Delete Item Access Control

| Item ID Exists | Item Belongs to Current User | ID is Numeric | Expected Result |
|---|---|---|---|
| Yes | Yes | Yes | Item deleted from `tracked_items`, confirmation displayed |
| Yes | No | Yes | Rejection message: `"❌ Item not found or it doesn't belong to you."` |
| No | N/A | Yes | Rejection message: `"❌ Item not found or it doesn't belong to you."` |
| N/A | N/A | No | Validation error: `"❌ Invalid ID. Please send a number."` |