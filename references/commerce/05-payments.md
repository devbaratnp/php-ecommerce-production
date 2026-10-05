# Payments

## Scope

This reference defines architecture and UX integration rules, not provider-specific API code. Always read the payment provider's current official integration and webhook documentation before implementation.

## Payment method categories

Possible project methods:

- Cash on Delivery (COD);
- manual bank transfer/QR;
- wallet/gateway (eSewa, Khalti, etc. depending on market);
- card gateway;
- store credit/gift card;
- mixed/partial methods if explicitly supported.

Do not display a payment option merely because there is a logo asset. It must have a complete operational flow.

## Payment state machine

Avoid boolean `paid` as the only state when gateways exist.

Typical states:

```text
created
pending
requires_action
authorized
captured/paid
failed
cancelled
partially_refunded
refunded
```

Provider terms differ. Map them carefully rather than inventing equivalence.

## Online payment flow

Conceptual redirect flow:

```text
Checkout
 -> create local order/payment attempt in pending state
 -> create/initialize provider payment
 -> redirect/present provider UI
 -> provider callback/redirect
 -> server verifies authoritative provider result
 -> webhook/reconciliation confirms final state
 -> update payment/order state idempotently
 -> confirmation
```

A browser success redirect alone is not sufficient evidence of payment.

## Webhooks

MUST:

- verify signature/authenticity using provider's current method;
- parse event type/version safely;
- find the expected payment/order;
- process idempotently;
- ignore/log duplicates correctly;
- handle out-of-order events where provider can send them;
- store provider reference;
- record enough audit data for reconciliation without storing prohibited sensitive data.

Return provider-required HTTP status after processing according to provider docs.

## Amount verification

When verifying a payment, check at minimum where supported:

- expected merchant/account;
- provider transaction/reference;
- amount;
- currency;
- order/reference association;
- final provider state.

Do not trust amount from query parameters.

## Manual QR/bank payment

Define operational state clearly:

```text
Order placed
Payment: awaiting verification
```

If customers upload receipt/proof:

- use safe upload rules;
- restrict access;
- let admin verify/reject with audit trail;
- do not automatically mark paid simply because a file was uploaded.

## COD

COD is not "paid" at order placement.

Possible model:

```text
payment_method = COD
payment_status = unpaid/pending_collection
```

Payment may be recorded collected upon delivery/settlement according to operations.

## Payment UI contract

Each method should expose:

- recognizable name;
- concise explanation if unfamiliar;
- any fee (if lawful/applicable) before final action;
- whether payment happens now/later;
- method-specific CTA.

Examples:

```text
COD -> Place order
Gateway -> Pay Rs 4,250
External wallet -> Continue to eSewa
```

Do not label every CTA "Submit".

## Failure/retry

Payment failure must not automatically create duplicate orders.

Define:

- can the same pending order be retried?
- when does the payment attempt expire?
- when is stock reservation released?
- what does the customer see after cancel/timeout?
- how does admin reconcile "customer says paid but order pending"?

## Refunds

Refund flow should record:

- amount;
- currency;
- reason;
- order/payment relation;
- provider refund reference;
- actor;
- state;
- timestamps.

Validate cumulative refunds do not exceed captured amount.

Do not mark refund completed until provider/manual process is confirmed according to method.

## Security

Never store:

- CVV;
- raw full card details;
- provider secret keys in public JS;
- webhook secrets in repo.

Use hosted/tokenized payment fields according to provider requirements when handling cards.

## Reconciliation

Admin should be able to see:

- order amount;
- payment method;
- local payment state;
- provider reference;
- latest attempt;
- webhook/status history where relevant;
- mismatch flag.

A scheduled/manual reconciliation path is valuable for providers where network/webhook failure can leave ambiguous states.

## Payment acceptance tests

- [ ] Success is verified server-side.
- [ ] Duplicate webhook does not duplicate effects.
- [ ] Tampered amount/reference fails verification.
- [ ] Failed/cancelled payment has recoverable UX.
- [ ] Retry does not unexpectedly duplicate order.
- [ ] COD is not falsely marked paid.
- [ ] Refund limits and states are enforced.
- [ ] Secrets/card data are not exposed/logged.
