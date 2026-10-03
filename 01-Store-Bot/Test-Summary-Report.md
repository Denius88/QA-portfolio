# Test Summary Report: Store Bot

| Field | Value |
|---|---|
| **Author** | Denis Pelekh |
| **Role** | QA Engineer |
| **Test period** | 02.10.2026 |
| **Build / version** | v1.0-beta |
| **Environment** | Local, macOS, Telegram, SQLite |
| **Report status** | Final |

## 1. Executive Summary

Manual testing of the Store Bot was performed to verify the main customer flow, checkout, order management, and administrator access control.

**Overall result:** Passed

**Release recommendation:** Ready for release within the tested scope.

## 2. Scope of Testing

### Tested Areas

- Customer registration and `/start` command.
- Catalog navigation and category selection.
- Product details and product search.
- Cart operations and total calculation.
- Checkout and order creation.
- Order history and order details.
- Administrator access control.
- Product and category management.
- Order status management.
- Database backup.

### Not Tested

- Online payment processing.
- Load and performance testing.
- Production deployment.
- Telegram platform availability.
- Security penetration testing.

## 3. Test Execution Summary

| Test Area | Planned | Executed | Passed | Failed | Blocked | Notes |
|---|---:|---:|---:|---:|---:|---|
| Smoke testing | 9 | 9 | 9 | 0 | 0 | Customer critical path |
| Functional testing | 23 | 23 | 23 | 0 | 0 | Includes customer and admin flows |
| Negative testing | 7 | 7 | 7 | 0 | 0 | Included in the functional test cases |
| Exploratory testing | 2 | 2 | 2 | 0 | 0 | Cart/checkout and admin/order sessions |
| Regression testing | 12 | 12 | 12 | 0 | 0 | Critical customer and admin flows |
| **Unique test cases** | **23** | **23** | **23** | **0** | **0** | **Smoke and negative tests are subsets** |

## 4. Test Results

### Passed Scenarios

- All customer-flow test cases passed.
- Cart functionality test cases passed.
- Checkout created an order and cleared the cart correctly.
- Admin control panel worked correctly.
- Order status updates worked for administrator and user.
- Database backup was created successfully.


### Failed Scenarios

- None.

### Blocked Scenarios

- None.

## 5. Defects Summary

| Bug ID | Title | Severity | Priority | Status |
|---|---|---|---|---|
| - | No defects found during the executed test scope | - | - | - |

## 6. Risks and Limitations

- Testing was performed in a local environment only.
- Online payments and load testing were outside the scope.
- Telegram Desktop behavior was tested; mobile behavior was not separately verified.
- Testing was performed through two exploratory sessions covering cart/checkout and admin/order management.
- Regression testing covered 12 critical customer and administrator checks; all passed.

## 7. Test Environment and Evidence

- **Operating system:** macOS
- **Client:** Telegram Desktop
- **Application environment:** Local
- **Database:** SQLite
- **Evidence:** See the `evidence/` directory for eight execution screenshots.
- **Automated test results:** `pytest -q` - 2 passed in 0.18s

## 8. Conclusion

Based on the executed tests, the Store Bot is ready for release within the tested scope.

The main customer flow, checkout, order management, admin access control, and database backup worked as expected. No defects were found during the executed test scope. The limitations listed above should be considered before a production release.

## 9. Recommendations

- Add automated tests for checkout validation and admin flows.
- Repeat exploratory testing before future releases.
- Extend evidence coverage when new features are added.
- Repeat the regression checklist after future catalog or order-flow changes.

