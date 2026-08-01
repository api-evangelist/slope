---
name: Create a Slope order and drive checkout
description: Create a Slope order and drive checkout
api: openapi/slope-v4-openapi.json
operations:
- createOrder
- getOrder
- openOrder
- finalizeOrder
---

# Create a Slope order and drive checkout

## Auth
HTTP Basic: `Authorization: Basic base64(public_key:secret_key)` with your **secret** key server-side. Sandbox `https://api.sandbox.slopepay.com/v4`, production `https://api.slopepay.com/v4`.

## Steps
1. **createOrder** (`POST /v4/orders`) with `amount` (cents), `currency` (e.g. `usd`), a unique `externalId`, and an `idempotency-key` header. Response returns `id`, `checkoutCode`, `checkoutUrl`.
2. Drive checkout on the frontend: pass `checkoutCode` to the Slope.js modal OR redirect the customer to `checkoutUrl`.
3. Order moves `approved -> open` when the customer completes the widget. Listen for `so.slope.order.opened` or poll **getOrder** (`GET /v4/orders/{id}`).
4. **finalizeOrder** (`POST /v4/orders/{id}/finalize`) once confirmed. Payout follows finalization; terminal. Slope may auto-finalize. Use **openOrder** only if you use the `submitted` status.

## Rules
- Send `idempotency-key` on every POST; a duplicate returns 409. Duplicate `externalId` returns 409 `conflictingExternalId`.
- Finalize or cancel open orders promptly (0-3 days). A finalized order cannot be cancelled - refund via `createOrderAdjustment`.
- Errors: `{statusCode, message, error}`; log `X-Amzn-RequestId`.
