# Smoke Test Checklist - Store Bot (Customer Flow)

| ID | Action | Expected Result | Status (Pass/Fail) | Comment |
|---|---|---|---|---|
| CK-01 | Main menu -> Catalog | All 3 categories are displayed (Electronics, Books, Clothing) | Pass | All 3 categories are displayed. |
| CK-02 | Electronics category | Test Phone and Test Laptop are displayed | Pass | Test Phone and Test Laptop are displayed. |
| CK-03 | Test Phone product card | The product name, price (11,000), and description are displayed | Pass | The Test Phone name, price, and description are correct. |
| CK-04 | Add a product to the cart | The button works and a confirmation message is displayed | Pass | The product was added and the bot displayed a confirmation message. |
| CK-05 | View the cart | Test Phone is displayed with quantity 1 and total 11,000 | Pass | The product price and quantity are displayed correctly. |
| CK-06 | Increase quantity (+) | The quantity changes to 2 and the total is recalculated as 22,000 | Pass | The quantity changed to 2 and the total was recalculated. |
| CK-07 | Decrease quantity (-) | The quantity changes from 2 to 1 and the total is recalculated as 11,000 | Pass | The quantity was decreased to 1 and the cart total was recalculated. |
| CK-08 | Remove a product | The product is removed and the cart becomes empty | Pass | The product was removed and the cart became empty. |
| CK-09 | Place an order | The bot requests contact details and creates the order | Pass | The order was created and the cart was cleared. |