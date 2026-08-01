---
name: Estimate and repay a Slope order
description: Estimate and repay a Slope order
api: openapi/slope-v4-openapi.json
operations:
- getOrderRepayEstimate
- repayOrderFull
---

# Estimate and repay a Slope order

## Auth
HTTP Basic with your secret key, PLUS a `Slope-Link-Token` header (from the customer Link Flow), required on repayment endpoints.

## Steps
1. **getOrderRepayEstimate** (`GET /v4/orders/{id}/repay-estimate`) - requires `Slope-Link-Token`.
2. **repayOrderFull** (`POST /v4/orders/{id}/repay-full`) - requires `Slope-Link-Token`; send `idempotency-key`.
3. Listen for `so.slope.transactions.updated` when a repayment happens.

## Rules
- Do not cache the repay estimate; amounts change dynamically.
- `Slope-Link-Token` is mandatory here in addition to Basic auth.
