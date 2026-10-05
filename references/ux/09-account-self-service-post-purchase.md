# Account, Self-Service, and Post-Purchase UX

## Why this is a primary UX domain

Purchase completion is not the end of ecommerce. Returning customers need fast access to orders, tracking, cancellation, returns, refunds, addresses, and support.

Research basis: Baymard's 2026 Accounts & Self-Service research found order management/tracking is a major customer priority and that hiding order-item details makes order identification unnecessarily difficult.

## Account dashboard

Prioritize tasks, not decorative welcome cards.

Useful entries:

- recent orders;
- tracking alerts;
- reorder where relevant;
- addresses;
- profile/security;
- returns/refunds;
- wishlist/saved items if enabled;
- support.

Do not make customers navigate five levels to find orders.

## Order list contract

Each order card/row should expose enough to identify it without opening several records:

- order number;
- order date;
- representative product image/name or item list summary;
- total;
- current order/fulfillment status;
- expected delivery when applicable;
- key action: View / Track / Buy again.

Example:

```text
Order #NP-48291                      Sep 20, 2026
[img] LMM Dairy Ghee 1L ×2          Rs 2,200
Shipped · Expected Sep 23
[Track order] [View details] [Buy again]
```

## Order detail

Show:

- all purchased items/options;
- prices/totals;
- shipping/contact snapshot;
- payment method/state;
- fulfillment/tracking;
- status timeline/events when useful;
- invoice/receipt where supported;
- eligible cancel/return/support actions.

Do not expose staff-only internal notes.

## Tracking

Integrate useful tracking information in the store when data is available:

```text
Confirmed           ✓
Processing          ✓
Packed              ✓
Shipped             ●
Out for delivery    ○
Delivered           ○

Expected Sep 24
Courier: ...
Tracking: ...
```

A courier external link can supplement this, not replace all useful status context.

## Guest tracking

Support guest self-service where feasible.

Do not require the user to create an account after purchase simply to see an order they already own.

Use a secure ownership mechanism such as:

- unguessable emailed order token; or
- order number + verified contact challenge appropriate to risk.

Do not expose private order data using sequential order number alone.

## Cancellation

If cancellation is allowed:

- show action only while eligible;
- explain consequences;
- confirm destructive action;
- show resulting refund/payment state separately;
- surface when cancellation is no longer possible and why.

## Returns

Order detail should tell customers whether each item is returnable.

Do not simply make the Return button disappear after the window.

Prefer:

```text
Return available until Oct 14
```

or:

```text
Return period ended Sep 10
[View return policy]
```

Return flow:

```text
Select item/qty -> reason -> resolution -> method/instructions -> confirm -> track
```

## Refund visibility

Show financial state separately from logistics:

```text
Return received
Refund of Rs 1,200 initiated Sep 28
Expected to original payment method in ...
```

Only promise timing supported by payment/business process.

## Reorder / buy again

Useful for grocery/FMCG/consumables and repeat B2B.

Reorder must:

- re-add current variants/products if still available;
- use current prices;
- respect changed pack sizes/options;
- clearly report unavailable lines.

Do not duplicate the previous order blindly with stale prices/stock.

## Addresses

Allow:

- add/edit/delete;
- default address if useful;
- clear labels;
- confirmation before deletion when in use.

Editing account address must not rewrite historical orders.

## Profile/security

Support:

- email/phone changes with verification as appropriate;
- password change;
- session/device management if implemented;
- MFA/passkeys/OTP where project risk justifies them;
- delete/deactivate account according to policy/legal obligations.

Do not block password managers, paste, or browser credential tools.

## Notifications

Transactional messages may include:

- order confirmation;
- payment confirmation/failure;
- shipped/tracking;
- delivery;
- return/refund state.

Every notification should link to a valid secure destination and reflect authoritative state.

Marketing consent is separate from transactional communication.

## Post-checkout account creation

After guest order:

```text
Create a password to save this order and check out faster next time.
```

Only when linking can be done securely and transparently.

Do not silently subscribe to marketing while creating the account.

## Self-service acceptance checks

- [ ] Order list exposes product/status context without opening each order.
- [ ] Order detail shows accurate item/payment/fulfillment state.
- [ ] Guest can access supported post-purchase info securely.
- [ ] Tracking is understandable within the storefront.
- [ ] Cancellation/returns expose eligibility and reasons.
- [ ] Refund status is separate from return status.
- [ ] Reorder reprices and revalidates current catalog.
- [ ] Account address edits do not change order snapshots.
- [ ] Transactional notification links are valid and secure.
