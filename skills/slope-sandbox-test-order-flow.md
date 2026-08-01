---
name: Simulate an approved order end-to-end in the Slope sandbox
description: Simulate an approved order end-to-end in the Slope sandbox
api: openapi/slope-v4-openapi.json
operations:
- simulateCreateCustomer
- simulateUpdateCustomerEligibility
- createOrder
- simulateApproveOrder
- simulateRejectOrder
---

# Simulate an approved order end-to-end in the Slope sandbox

## Auth
HTTP Basic with your sandbox secret key (`sk_sndbx_...`) at `https://api.sandbox.slopepay.com/v4`.

## Steps
1. **simulateCreateCustomer** (`POST /v4/simulation/customer`) - test customer with a user password.
2. **simulateUpdateCustomerEligibility** (`POST /v4/simulation/customer/eligibility`) - set eligibility status and credit limit.
3. **createOrder** (`POST /v4/orders`) with `amount`, `currency: usd`, unique `externalId`, `idempotency-key`. Amount `133799` ($1,337.99) triggers a rejection.
4. **simulateApproveOrder** (`POST /v4/simulation/approve-order`) or **simulateRejectOrder** (`POST /v4/simulation/reject-order`, terminal).

## Test values (sandbox)
- Card `4242424242424242` succeeds; `4544249167673670` fails.
- Plaid `custom_high_bal` passes pre-qual ($100k); `custom_low_bal` rejects.
- Sandbox orders are approved by default unless a rejection is triggered.

## Rules
- Simulation endpoints are sandbox-only; never call them against production.
