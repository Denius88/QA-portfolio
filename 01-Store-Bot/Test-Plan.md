# Test Plan: Store Bot

| Metadata | Details |
|---|---|
| **Author** | Denys Pelekh |
| **Role** | QA Engineer |
| **Document Version** | 1.0.0 |
| **Status** | Completed |
| **Target Build/Release** | v1.0-beta |
| **Last Updated** | 02.10.2026 |

## 1. Introduction and Objectives

Store Bot is a Telegram-based online store. Customers can browse the catalog, search for products, manage a cart, place orders, and view order history. Administrators can manage the catalog and update order statuses.

The purpose of this test plan is to define the scope, test approach, environment, test data, risks, and completion criteria for manual testing of Store Bot.

### Testing Objectives

- Verify the main customer flow: catalog -> product -> cart -> checkout -> order.
- Verify product search, pagination, order history, and status notifications.
- Verify that cart totals and order totals are calculated correctly.
- Verify checkout validation and cancellation behavior.
- Verify that only the configured administrator can access admin functions.
- Verify category, product, and order management in the admin panel.
- Verify that important data is saved correctly in SQLite.

## 2. Scope of Testing

### 2.1 In Scope

#### Customer Flow

- Start command and main menu.
- Catalog navigation and category selection.
- Product details, descriptions, prices, and optional photos.
- Product search, including empty and short queries.
- Catalog pagination when more than five products exist.
- Adding products to the cart.
- Increasing, decreasing, removing, and clearing cart items.
- Cart total calculation.
- Checkout with phone number and delivery address.
- Validation of invalid or too-short phone numbers and addresses.
- Checkout cancellation.
- Order creation and cart clearing after a successful order.
- Viewing order history and order details.
- Receiving order status updates.
- Contacts screen.

#### Admin Flow

- Access to the admin panel using the configured administrator ID.
- Rejection of admin commands for regular users.
- Adding categories and products.
- Product photo upload or skipping a photo.
- Editing categories and products.
- Positive price validation and category selection.
- Soft deletion of products.
- Deletion restrictions for categories that still contain products.
- Viewing all orders and ordered items.
- Changing order status to confirmed, processing, delivered, or cancelled.
- Creating a database backup with `/backup`.

#### Data and Logic Layer

- User creation and identification by Telegram ID.
- Cart persistence and quantity updates.
- Order creation from cart contents.
- Preservation of product price and quantity in order items.
- Clearing the cart after order creation.
- User access to only their own order details.
- Integrity of relationships between users, products, carts, and orders.

### 2.2 Out of Scope

- Online payment providers and payment processing.
- Inventory or stock management, because it is not implemented.
- Performance and load testing for more than 100 concurrent users.
- Telegram platform availability and Telegram client defects.
- Production deployment monitoring.
- Security penetration testing.

## 3. Test Strategy and Test Types

| Test Type | Focus Area | Technique / Approach |
|---|---|---|
| Smoke Testing | Main bot flow and application availability | Manual checklist |
| Functional Testing | Customer flow, admin flow, cart, checkout, and orders | Detailed test cases |
| Negative Testing | Invalid input, unauthorized access, missing data | Negative test cases |
| Boundary Testing | Short phone numbers, short addresses, zero or negative prices, long text | Boundary value analysis |
| Exploratory Testing | Unexpected user behavior and unplanned defects | Time-boxed sessions with test charters |
| Data Integrity Testing | Cart, order, and user records | Database checks and automated tests |
| Regression Testing | Previously tested critical functionality after changes | Smoke checklist and selected test cases |

Testing will be performed manually through Telegram. Existing automated database tests will be used as supporting evidence for data and business logic checks.

## 4. Test Environment and Test Data

### 4.1 Environment

- **Application:** Store Bot
- **Client:** Telegram Desktop; mobile behavior may be checked separately when available.
- **Host OS:** macOS
- **Runtime:** Python 3.13+
- **Frameworks:** aiogram 3, SQLAlchemy 2, aiosqlite
- **Database:** Local SQLite test database
- **Deployment:** Local development environment
- **Test mode:** Manual functional testing with supporting automated database tests

### 4.2 Test Users

- Regular Telegram user for customer scenarios.
- Configured administrator Telegram user for admin scenarios.
- Separate regular user to verify unauthorized admin access and order privacy.

### 4.3 Test Data

- Categories: Electronics, Books, Clothing.
- Products: Test Phone, Test Laptop, and at least one product with a photo.
- Prices: positive whole numbers and decimal values.
- Invalid prices: `0`, negative values, text, and empty input.
- Valid phone number: `+380000000000`.
- Invalid phone values: empty input, short values, and text shorter than seven characters.
- Valid address: `Test address, Kyiv`.
- Invalid address: empty input and text shorter than five characters.
- Orders in each supported status: pending, confirmed, processing, delivered, cancelled.

## 5. Entry and Exit Criteria

### 5.1 Entry Criteria

- The application starts without critical errors.
- Dependencies are installed and the local database is available.
- A valid test bot token and administrator ID are configured locally.
- Test categories and products are available in the database.
- A regular Telegram test user and an administrator test user are available.
- Test cases and the smoke checklist are prepared.

### 5.2 Exit Criteria

- All smoke tests have been executed.
- The main catalog-to-order flow works successfully.
- No open Blocker or Critical defects remain.
- All failed tests have linked bug reports or documented explanations.
- Cart totals and order totals have been verified.
- Admin access control and order privacy have been verified.
- Test results and remaining risks are documented in the Test Summary Report.

## 6. Risk Management

| Risk ID | Risk Description | Probability | Impact | Mitigation Strategy |
|---|---|---|---|---|
| R-01 | Incorrect cart or order total can lead to incorrect customer charges or order data. | Medium | High | Verify totals with different quantities and products; compare UI data with database records. |
| R-02 | Unauthorized users may access admin functionality. | Low | High | Test every admin entry point with a regular user. |
| R-03 | Invalid checkout data may create incomplete or unusable orders. | Medium | High | Execute negative and boundary tests for phone and address fields. |
| R-04 | Local SQLite data may be lost or corrupted. | Low | High | Verify startup backup and manual `/backup` behavior. |
| R-05 | Changes to catalog or order status may break existing customer flows. | Medium | Medium | Run regression smoke tests after admin changes. |

## 7. Deliverables

- Test Plan.
- Smoke Test Checklist.
- Functional and negative Test Cases.
- Exploratory Testing notes.
- Bug Reports with severity, priority, steps, expected result, and actual result.
- Test Summary Report.
- Screenshots or other execution evidence.
- Automated database test results as supporting evidence.