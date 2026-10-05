# Gifting and Subscription Commerce

Optional patterns requiring additional business rules beyond ordinary one-time retail.

## Gifting

Potential features:

- gift message;
- hide price on packing slip;
- gift wrap;
- gift receipt;
- alternate recipient;
- scheduled delivery;
- gift card.

Each visible option requires operational fulfillment support.

### Gift address

Do not assume buyer and recipient are the same person.

Contact model may need:

- purchaser email/phone;
- recipient name/address/phone;
- communication preference so order details are not accidentally sent to recipient early.

### Gift cards

If sold/stored value:

- unique secure code;
- balance ledger;
- activation state;
- redemption limits;
- expiry according to law/business rule;
- fraud protections;
- refund handling;
- partial redemption.

Do not implement gift cards as ordinary coupon codes if stored-value/accounting requirements differ.

## Subscriptions / recurring orders

Subscription adds a lifecycle separate from the order itself.

Model concepts:

- plan/frequency;
- subscribed product/variant;
- quantity;
- next billing/order date;
- payment method token/reference;
- status;
- pause/resume/cancel rules;
- skip next;
- address changes;
- price change policy.

### Subscription UX

The purchase choice must make recurring nature unambiguous:

```text
One-time purchase: Rs 1,000
Subscribe every 4 weeks: Rs 900 / delivery
```

Do not preselect subscription deceptively.

Before checkout/final action disclose:

- frequency;
- recurring amount or pricing rule;
- cancellation/pause policy;
- first charge/order timing.

### Self-service

Customers should be able to see:

- active subscription;
- next shipment/date;
- product/quantity;
- price;
- address;
- payment method;
- skip/pause/cancel actions allowed by policy.

Do not require customer support for every routine subscription change when the business model supports self-service.

### Failed renewal

Define:

- payment retry schedule;
- customer notification;
- stock handling;
- grace period;
- subscription suspension/cancellation.

Avoid creating repeated unpaid orders without a clear policy.

## Acceptance checks

- [ ] Gift options have fulfillment support.
- [ ] Recipient/buyer communication is intentional.
- [ ] Gift-card balance cannot be overspent.
- [ ] Subscription recurring nature is explicit before purchase.
- [ ] Renewal/payment failure lifecycle is defined.
- [ ] Customer can view/manage supported subscription actions.
