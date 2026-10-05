# Admin Operations Workflows

## Goal

Map the back-office workflows required to operate the customer experience. A storefront feature should not ship if staff have no reliable way to manage its resulting state.

## Product workflow

Typical:

```text
Create draft
 -> add identity/content
 -> add variants/SKUs
 -> add media
 -> set pricing
 -> set inventory
 -> assign taxonomy/attributes
 -> preview
 -> publish
 -> update/archive
```

Validation before publish may require:

- name;
- slug;
- at least one valid purchasable variant;
- price;
- primary image depending on business;
- category;
- stock/backorder policy.

Do not force every field to be required when business does not need it.

## Variant matrix

For apparel-like products, an admin variant matrix can speed editing:

```text
           S       M       L
Black    SKU     SKU     SKU
White    SKU     SKU     SKU
```

Allow bulk price/stock operations carefully while preserving unique SKUs.

## Inventory operations

Support applicable:

- initial stock;
- manual adjustment with reason;
- receive stock;
- transfer location;
- cycle count/reconciliation;
- low-stock thresholds;
- movement history.

Never let staff type a new stock number without recording why when inventory traceability matters.

## Order queue

Default order list should help staff find orders needing action.

Useful saved views:

- New/Pending
- Payment pending verification
- Paid/unfulfilled
- Ready to pack
- Shipped
- Failed/problem orders
- Return requested

Do not bury action queues behind a dashboard chart.

## Order detail workflow

Possible actions:

```text
confirm
mark processing
pack
assign courier
add tracking
mark shipped
mark delivered (manual only if appropriate)
cancel
refund
add internal note
contact customer
```

Actions shown depend on current state and permission.

## State transition validation

The UI must not offer impossible transitions.

Example:

- delivered order should not casually return to `pending`;
- refund action only when refundable captured payment exists;
- shipped order cancellation may require return/interception process rather than simple status change.

Server enforces the same rules.

## Packing workflow

For higher-volume stores consider:

- pick list;
- barcode/SKU scan;
- quantity confirmation;
- print packing slip/label;
- package weight;
- courier selection;
- tracking creation.

Small stores may keep this simpler but still need clear packed/shipped states.

## Courier integration

If API integrated:

- admin sees request status;
- failed booking can be retried idempotently;
- tracking/reference stored;
- label available;
- webhook/status changes reconcile safely;
- provider errors are readable to staff, not customers.

Manual fallback may be required.

## Customer support notes

Internal notes:

- clearly labeled staff-only;
- actor/time recorded;
- never shown in customer order response/template.

Customer-visible messages should be a separate channel/data field.

## Payment verification

For manual bank/QR:

Queue should expose:

- order;
- expected amount;
- submitted proof if any;
- customer reference;
- Verify / Reject;
- reason/note;
- audit trail.

Verification changes payment state and may unlock fulfillment according to business rules.

## Refund workflow

```text
Select refundable order/payment
 -> choose lines/amount
 -> reason
 -> preview refund total
 -> permission/confirmation
 -> provider/manual refund
 -> record result
 -> notify customer
```

Handle provider failure without falsely marking refunded.

## Return workflow

Queue:

```text
Requested
Approved / Rejected
Received
Inspected
Resolution: Refund / Exchange / Credit
Completed
```

Restock decision should be explicit when item condition matters.

## Bulk actions

Good candidates:

- export selected;
- assign category/tag;
- archive products;
- print selected labels.

High-risk candidates should be limited or require strong confirmation:

- bulk refunds;
- bulk delete;
- mass price changes;
- mass stock overwrite.

## Reports

Operational reports should be based on authoritative order/payment states.

Clearly define whether revenue means:

- orders placed;
- paid orders;
- net after refunds;
- delivered orders.

Avoid dashboards where metric definition is ambiguous.

## Data export

- permission-protected;
- respect privacy/PII minimization;
- escape CSV formula injection risks;
- log sensitive exports where appropriate;
- use streaming/chunking for large data sets.

## Operations acceptance checks

- [ ] Every customer-facing state has an admin management path.
- [ ] Order queue makes actionable work visible.
- [ ] Invalid status transitions are blocked in UI and server.
- [ ] Manual payment verification is auditable.
- [ ] Refund UI cannot exceed refundable amount.
- [ ] Stock adjustments preserve reason/history where required.
- [ ] Courier failure has recovery path.
- [ ] Staff-only notes cannot leak to customers.
- [ ] Report metric definitions are explicit.
