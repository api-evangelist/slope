---
name: Refund or partially cancel a Slope order
description: Refund or partially cancel a Slope order
api: openapi/slope-v4-openapi.json
operations:
- partialCancelOrder
- createOrderAdjustment
- getOrderAdjustment
---

# Refund or partially cancel a Slope order

## Auth
HTTP Basic with your secret key.

## Steps
- Before finalization: reduce with **partialCancelOrder** (`POST /v4/orders/{id}/partial-cancel`). Cancelled amount posts as a credit once finalized; original `total` unchanged.
- After finalization: cancellation is not allowed. Refund with **createOrderAdjustment** (`POST /v4/orders/{orderId}/adjustments`), adjustment type refund.
- Track via **getOrderAdjustment** (`GET /v4/orders/{orderId}/adjustments/{id}`). Listen for `so.slope.order.adjustment.created`.

## Rules
- Send `idempotency-key` on POSTs. A finalized order is terminal and can only be adjusted.
