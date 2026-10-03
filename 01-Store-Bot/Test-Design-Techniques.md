# Test Design Techniques: Store Bot

This document explains how test design techniques were applied to the Store Bot test scenarios.

## Boundary Value Analysis

Boundary values were applied to fields with minimum or meaningful limits.

| Field | Invalid boundary | Valid boundary | Expected behavior |
|---|---|---|---|
| Phone number | Fewer than 7 characters | 7 or more characters | Short values are rejected; valid-length values advance checkout. |
| Delivery address | Fewer than 5 characters | 5 or more characters | Short values are rejected; valid values advance checkout. |
| Product price | `0` or a negative value | Positive number greater than `0` | Non-positive prices are rejected. |
| Search query | 1 character | 2 or more characters | Short queries are rejected; valid queries return results or `No products found`. |

Related test cases: `TC-006`, `TC-013`, `TC-014`, and `TC-021`.

## Equivalence Partitioning

Input values were divided into groups that should produce the same behavior.

| Input | Valid partition | Invalid partitions |
|---|---|---|
| Phone number | 7 or more characters | Empty value, fewer than 7 characters |
| Address | 5 or more characters | Empty value, fewer than 5 characters |
| Product price | Positive numeric value | Zero, negative number, non-numeric text |
| Search query | 2 or more characters with or without matches | Empty value or one-character query |
| User role | Configured administrator | Regular user attempting admin actions |

## State Transition: Cart and Order Flow

```text
Empty cart
  -> Product added
  -> Quantity increased or decreased
  -> Checkout started
  -> Phone and address accepted
  -> Order confirmed
  -> Cart cleared
  -> Order status: pending
  -> confirmed / processing
  -> delivered or cancelled
```

Important transition checks:

- Checkout cannot start from an empty cart.
- Removing the last item returns the cart to the empty state.
- Confirming an order clears the cart.
- A regular user cannot transition into the admin flow.

Related test cases: `TC-008` through `TC-018` and `TC-019` through `TC-022`.

## Decision Table: Checkout Availability

| Cart state | Phone valid | Address valid | Expected result |
|---|---|---|---|
| Empty | Any | Any | Checkout is rejected. |
| Has items | No | Any | Phone validation error is displayed. |
| Has items | Yes | No | Address validation error is displayed. |
| Has items | Yes | Yes | Confirmation step is displayed and the order can be created. |
