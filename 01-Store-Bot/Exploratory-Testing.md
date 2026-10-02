# Exploratory Testing: Store Bot

| Field | Value |
|---|---|
| **Tester** | Denis Pelekh |
| **Role** | QA Engineer |
| **Application** | Store Bot |
| **Environment** | Local, macOS, Telegram, SQLite |

## What Is Exploratory Testing?

Exploratory testing was performed using test charters. The tester explored the selected functionality while designing and executing checks at the same time. The sessions were not limited to the predefined test cases.

## Session 1: Cart and Checkout

| Field | Value |
|---|---|
| **Date** | 02.10.2026 |
| **Duration** | Approximately 30 minutes |
| **Tester** | Denis Pelekh |
| **Charter** | Explore cart operations and the checkout flow from a customer perspective. |
| **Test data** | Test Phone, Test Book, valid phone number, and valid delivery address |

### Areas Explored

- Adding one product to the cart.
- Adding the same product more than once.
- Increasing and decreasing product quantity.
- Removing a product from the cart.
- Clearing the cart.
- Starting checkout with an empty cart.
- Entering invalid and valid phone numbers.
- Entering invalid and valid delivery addresses.
- Cancelling checkout at different steps.
- Confirming an order and checking that the cart is cleared.

### Notes and Observations

- The cart quantity and total were recalculated correctly after increasing and decreasing the quantity.
- The product was removed correctly, and the empty-cart message was displayed.
- Checkout could not be started with an empty cart.
- Invalid phone numbers and short addresses were rejected with validation messages.
- A valid order was created successfully, and the cart was cleared after confirmation.

### Result

- **Result:** Completed
- **Issues found:** None
- **Evidence:** None

## Session 2: Admin Panel and Order Management

| Field | Value |
|---|---|
| **Date** | 02.10.2026 |
| **Duration** | Approximately 30 minutes |
| **Tester** | Denis Pelekh |
| **Charter** | Explore admin access, catalog management, order management, and backup behavior. |
| **Test data** | Administrator account, regular account, test category, test product, and test order |

### Areas Explored

- Opening the admin panel as an administrator.
- Trying to open the admin panel as a regular user.
- Creating and editing a category.
- Creating and editing a product.
- Entering invalid product prices.
- Uploading a product photo or skipping the photo.
- Viewing customer orders as an administrator.
- Changing order statuses.
- Checking customer status notifications.
- Creating a database backup with `/backup`.

### Notes and Observations

- Admin access was available only to the user configured in `.env`.
- Regular users could not access the admin panel.
- The product was created and displayed in its category with the correct details.
- Order statuses were updated for the administrator and the customer.
- The database backup was created successfully in the `backups` folder.

### Result

- **Result:** Completed
- **Issues found:** None
- **Evidence:** None

## Overall Exploratory Testing Result

- **Sessions completed:** 2
- **Total duration:** Approximately 60 minutes
- **Issues found:** None
- **General conclusion:** The explored customer and administrator flows worked as expected within the tested scope.
