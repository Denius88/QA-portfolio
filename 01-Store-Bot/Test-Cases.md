
# Test Cases: Store Bot

| Field | Value |
|---|---|
| Application | Store Bot |
| Author | Denis Pelekh |
| Role | QA Engineer |
| Environment | Local, macOS, Telegram |
| Test data | Electronics, Books, Clothing; Test Phone; Test Laptop, Test Book, Test T-Shirt |

## Customer Flow

### TC-001 - Start the bot

- **Priority:** High
- **Type:** Functional
- **Preconditions:** The bot is running and the test user has access to Telegram.
- **Steps:**
	1. Open the bot chat.
	2. Send `/start`.
- **Expected result:** The bot creates or updates the user profile and displays the main menu.
- **Actual result:** After sending `/start`, a welcome message was displayed and the user could navigate the menu.
- **Status:** Pass

### TC-002 - Open the catalog

- **Priority:** High
- **Type:** Functional
- **Preconditions:** The test database contains at least one active category.
- **Steps:**
	1. Open the main menu.
	2. Select `Catalog`.
- **Expected result:** The bot displays the available product categories.
- **Actual result:** Clicking the `Catalog` button displayed the available categories.
- **Status:** Pass

### TC-003 - View products in a category

- **Priority:** High
- **Type:** Functional
- **Preconditions:** The `Electronics` category contains `Test Phone` and `Test Laptop`.
- **Steps:**
	1. Open `Catalog`.
	2. Select `Electronics`.
- **Expected result:** Active products from the selected category are displayed.
- **Actual result:** The user could see the available products in the `Electronics` category.
- **Status:** Pass

### TC-004 - View product details

- **Priority:** Medium
- **Type:** Functional
- **Preconditions:** `Test Phone` exists and is active.
- **Steps:**
	1. Open the `Electronics` category.
	2. Select `Test Phone`.
- **Expected result:** The product name, description, price, optional photo, and add-to-cart action are displayed.
- **Actual result:** The user could see the product information, including the name, description, price, photo, and add-to-cart button.
- **Status:** Pass

### TC-005 - Search for an existing product

- **Priority:** High
- **Type:** Functional
- **Preconditions:** An active product named `Test Phone` exists.
- **Steps:**
	1. Select `Search`.
	2. Enter `Phone`.
- **Expected result:** Matching active products are displayed.
- **Actual result:** The user could see the list of available products that contain `Phone` in their name.
- **Status:** Pass

### TC-006 - Search with a short query

- **Priority:** Medium
- **Type:** Negative / Boundary
- **Preconditions:** The bot is displaying the search prompt.
- **Steps:**
	1. Enter one character, for example `P`.
- **Expected result:** The bot rejects the query and asks for at least two characters.
- **Actual result:** The message `Please enter at least 2 characters` was displayed.
- **Status:** Pass

### TC-007 - Search for a missing product

- **Priority:** Medium
- **Type:** Negative
- **Preconditions:** The bot is displaying the search prompt.
- **Steps:**
	1. Enter `NonExistingProduct`.
- **Expected result:** The bot displays a clear `No products found` message.
- **Actual result:** The message `No products found` was displayed.
- **Status:** Pass

### TC-008 - Add a product to the cart

- **Priority:** High
- **Type:** Functional
- **Preconditions:** An active product is available.
- **Steps:**
	1. Open the product details.
	2. Select `Add to cart`.
	3. Open `Cart`.
- **Expected result:** The product is present in the cart with quantity `1` and the correct subtotal.
- **Actual result:** The product was added to the cart with quantity `1` and the correct total price.
- **Status:** Pass

### TC-009 - Increase product quantity

- **Priority:** High
- **Type:** Functional
- **Preconditions:** The cart contains one product with quantity `1`.
- **Steps:**
	1. Open the cart.
	2. Press the increase quantity button once.
- **Expected result:** The quantity changes to `2` and the subtotal and total are recalculated correctly.
- **Actual result:** The product quantity changed to `2`, and the price changed from `500` to `1,000`.
- **Status:** Pass

### TC-010 - Decrease product quantity

- **Priority:** High
- **Type:** Functional
- **Preconditions:** The cart contains one product with quantity `2`.
- **Steps:**
	1. Open the cart.
	2. Press the decrease quantity button once.
- **Expected result:** The quantity changes to `1` and the total is recalculated correctly.
- **Actual result:** The Test Book quantity changed to `1`, and the total price was `500`.
- **Status:** Pass

### TC-011 - Remove a product from the cart

- **Priority:** High
- **Type:** Functional
- **Preconditions:** The cart contains at least one product.
- **Steps:**
	1. Open the cart.
	2. Select the remove action for the product.
- **Expected result:** The product is removed from the cart and the empty-cart state is displayed when no items remain.
- **Actual result:** The product was removed, the cart became empty, and the message `Your cart is empty` was displayed.
- **Status:** Pass

### TC-012 - Start checkout with an empty cart

- **Priority:** High
- **Type:** Negative
- **Preconditions:** The cart is empty.
- **Steps:**
	1. Open the cart.
	2. Attempt to start checkout.
- **Expected result:** Checkout does not start and the bot informs the user that the cart is empty.
- **Actual result:** The user could not start checkout before adding at least one item to the cart.
- **Status:** Pass

### TC-013 - Reject an invalid phone number

- **Priority:** High
- **Type:** Negative / Boundary
- **Preconditions:** The checkout flow is waiting for a phone number.
- **Steps:**
	1. Enter a value shorter than seven characters, for example `123`.
- **Expected result:** The bot rejects the value and asks for a valid phone number without advancing to the address step.
- **Actual result:** The message `Please enter a valid phone number` was displayed.
- **Status:** Pass

### TC-014 - Reject an invalid delivery address

- **Priority:** High
- **Type:** Negative / Boundary
- **Preconditions:** A valid phone number was entered and checkout is waiting for an address.
- **Steps:**
	1. Enter a value shorter than five characters, for example `Home`.
- **Expected result:** The bot rejects the value and keeps the user on the address step.
- **Actual result:** The bot waited for a valid address containing at least 5 characters.
- **Status:** Pass

### TC-015 - Cancel checkout

- **Priority:** Medium
- **Type:** Functional
- **Preconditions:** Checkout is active.
- **Steps:**
	1. Select `Cancel` during phone, address, or confirmation step.
- **Expected result:** Checkout state is cleared, the user receives a cancellation message, and the main user menu is available.
- **Actual result:** Checkout was cleared and the user received the message `Checkout cancelled`.
- **Status:** Pass

### TC-016 - Create an order successfully

- **Priority:** Critical
- **Type:** Functional / Data Integrity
- **Preconditions:** The cart contains at least one product and valid phone and address data are available.
- **Steps:**
	1. Start checkout.
	2. Enter a valid phone number.
	3. Enter a valid delivery address.
	4. Confirm the order.
- **Expected result:** An order is created with status `pending`, the total matches the cart, the cart is cleared, and the user receives an order number.
- **Actual result:** The order was created with status `pending`, the cart was cleared, and the user received an order confirmation message.
- **Status:** Pass

### TC-017 - View order history

- **Priority:** Medium
- **Type:** Functional
- **Preconditions:** The user has at least one order.
- **Steps:**
	1. Select `My orders`.
	2. Open an order from the list.
- **Expected result:** The order list and selected order details display the correct status, products, quantities, and total.
- **Actual result:** The order list was displayed with the status, price, and ordered items.
- **Status:** Pass

### TC-018 - Verify order privacy

- **Priority:** Critical
- **Type:** Security / Functional
- **Preconditions:** User A has an order; User B does not own that order.
- **Steps:**
	1. Attempt to open User A's order using User B's account.
- **Expected result:** User B cannot view User A's order details.
- **Actual result:** User A could see only their own orders and could not view another user's order history.
- **Status:** Pass

## Admin Flow

### TC-019 - Reject admin access for a regular user

- **Priority:** Critical
- **Type:** Security / Negative
- **Preconditions:** The test user is not configured as the administrator.
- **Steps:**
	1. Send `/admin`.
	2. Try to use an admin menu action if one is visible.
- **Expected result:** The bot denies access and does not expose admin functionality.
- **Actual result:** The `/admin` command worked only for the user ID configured in `.env`; only that user could access the admin panel.
- **Status:** Pass

### TC-020 - Create and update a product as administrator

- **Priority:** High
- **Type:** Functional
- **Preconditions:** The test user has the configured administrator ID and at least one category exists.
- **Steps:**
	1. Open `/admin`.
	2. Add a product with a valid name, description, positive price, category, and optional photo.
	3. Edit the product price or description.
	4. Open the catalog as a regular user.
- **Expected result:** The product is created, updated values are saved, and the active product is visible to customers.
- **Actual result:** The product was created and displayed in its category with the correct price, name, description, and image.
- **Status:** Pass

### TC-021 - Reject an invalid product price

- **Priority:** High
- **Type:** Negative / Boundary
- **Preconditions:** The administrator is in the product creation or editing flow.
- **Steps:**
	1. Enter `0`.
	2. Repeat with a negative number and non-numeric text.
- **Expected result:** Each invalid value is rejected and the bot asks for a positive number without leaving the price step.
- **Actual result:** The message `The price must be a positive number` was displayed, and the bot waited for a valid price.
- **Status:** Pass

### TC-022 - Change an order status as administrator

- **Priority:** High
- **Type:** Functional
- **Preconditions:** The administrator has access to an existing order.
- **Steps:**
	1. Open `Manage orders`.
	2. Change the order status to `confirmed`.
	3. Repeat for `processing`, `delivered`, and `cancelled`.
- **Expected result:** The status is updated, the admin view refreshes, and the customer receives a status notification.
- **Actual result:** The order status was updated for both the administrator and the customer.
- **Status:** Pass

### TC-023 - Create a database backup

- **Priority:** Medium
- **Type:** Functional / Recovery
- **Preconditions:** The administrator is authorized and the database exists.
- **Steps:**
	1. Send `/backup`.
	2. Check the backup directory.
- **Expected result:** A backup file is created and the bot reports its filename.
- **Actual result:** The backup was successfully created and stored in the `backups` folder.
- **Status:** Pass
