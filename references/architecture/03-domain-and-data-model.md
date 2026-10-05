# Domain and Data Model

## Goal

Model ecommerce concepts so catalog changes do not corrupt historical orders, variants can carry their own stock/SKU/price, and operational flows can be audited.

The exact schema varies. This file describes responsibilities and invariants, not mandatory table names.

## Core aggregates

### Customer

Typical data:

- id/public id;
- name;
- email/phone as required;
- password hash if local auth;
- status;
- created/updated timestamps.

Keep addresses separate when customers may store multiple addresses.

### Product

Represents the customer-facing product concept.

Typical fields:

- id;
- slug;
- name;
- description/content;
- brand/category relation;
- status/draft/published;
- tax/shipping classification if applicable;
- SEO metadata where needed.

Do not put variant-specific SKU/stock into the product row if multiple variants can differ.

### Variant

Purchasable unit.

Typical fields:

- product_id;
- SKU;
- option values (size/color/weight/etc.);
- price or price override;
- compare-at price;
- barcode/GTIN where relevant;
- active state;
- weight/dimensions;
- stock policy.

The cart should generally reference a variant/purchasable ID, not only product ID.

### Category/taxonomy

Support:

- parent-child hierarchy if needed;
- ordering;
- slug;
- visibility;
- category-specific filters/attributes.

Avoid deep taxonomies simply because the schema permits them. UX should drive useful category depth.

### Product attributes

Separate attributes used for filtering/specifications from freeform description where the catalog demands faceted search.

Examples:

- brand;
- material;
- capacity;
- compatibility;
- size;
- color;
- weight class.

Use normalized or controlled values where filters depend on them.

### Media

Store metadata such as:

- product/variant relation;
- file/storage key;
- alt text;
- role (primary, gallery, detail, lifestyle, size/context);
- sort order;
- dimensions.

### Inventory

For simple single-location stock:

```text
inventory
- variant_id
- on_hand
- reserved (optional)
- updated_at
```

For multi-location:

```text
inventory_levels
- location_id
- variant_id
- on_hand
- reserved
```

Do not compute current stock by repeatedly summing the entire historical transaction ledger on every page request. Maintain current levels plus an audit ledger where needed.

### Cart

Support either:

- session cart for guest + DB cart for authenticated; or
- persistent cart table keyed by anonymous cart token and optional customer id.

Each cart item needs variant identity and quantity. Display price can be cached, but checkout must reprice authoritatively.

### Order

Order is a historical record, not a live view of current catalog data.

Store snapshots needed to reproduce what was purchased:

- order number/public id;
- customer id nullable for guest;
- contact snapshot;
- billing/shipping snapshot;
- currency;
- subtotal;
- discounts;
- shipping;
- tax;
- fees;
- grand total;
- order/payment/fulfillment state;
- created timestamp.

### Order item

Snapshot:

- source product/variant IDs for reference;
- product name at purchase time;
- SKU at purchase time;
- selected options;
- quantity;
- unit price;
- discount allocation if needed;
- tax amount if needed;
- line total.

A later product rename or price change must not rewrite order history.

### Payment

Do not use only `orders.payment_status` as the entire payment model when online payment exists.

Payment/attempt records may need:

- provider;
- provider transaction/reference;
- amount/currency;
- state;
- attempt number;
- idempotency key;
- raw provider payload reference/log (redacted);
- timestamps.

Never store full card credentials.

### Fulfillment/shipment

Track separately from payment when the business needs shipment lifecycle:

- carrier;
- tracking number;
- shipped_at;
- expected/estimated delivery where available;
- delivered_at;
- items/quantity if partial fulfillment is possible.

### Promotion/coupon

Model:

- code;
- type/value;
- active window;
- minimum amount;
- eligible categories/products/customers;
- usage limits;
- per-customer limits;
- combinability;
- status.

Validation occurs server-side at pricing time and again at checkout.

### Return/refund

Separate return request from refund money movement.

Return may contain:

- order item/quantity;
- reason;
- condition;
- requested resolution;
- status;
- received timestamp.

Refund records track financial action and provider reference.

## Status modeling

Avoid one giant `status` column trying to encode everything.

Prefer distinct dimensions:

- order/business status;
- payment status;
- fulfillment status;
- return/refund status.

For example a paid order can be unfulfilled, partially fulfilled, fulfilled, returned, or partially refunded.

## State transition rules

Do not allow arbitrary dropdown movement between all statuses.

Define allowed transitions, e.g.:

```text
pending -> confirmed -> processing -> packed -> shipped -> delivered
pending/confirmed -> cancelled
shipped -> delivered
```

Project-specific flows differ. The important rule is that transitions are explicit, validated, and audited.

## Keys and indexes

Common indexes:

- unique product slug;
- unique SKU;
- unique order number/public id;
- customer email if email uniquely identifies auth account;
- order customer_id + created_at;
- order status + created_at;
- inventory variant/location unique pair;
- cart owner/token;
- payment provider reference unique where appropriate.

Index based on observed query patterns, not every column.

## Foreign keys

Use foreign keys where operationally compatible to preserve referential integrity. Decide delete behavior intentionally.

Historical records often should not cascade-delete when a catalog item/customer is removed. Prefer soft deletion/archive or nullable references plus snapshots where history matters.

## Timestamps

Store system timestamps consistently, commonly UTC, then localize for display. Keep business-local dates explicit when rules depend on local calendar/date boundaries.

## Money

Use a consistent monetary representation.

For SQL, `DECIMAL` is common for currency values. Never use binary floating-point as the source of truth for order money.

Centralize rounding/allocation rules.

## Public identifiers

Avoid exposing sequential internal IDs where enumeration creates privacy/security problems, especially orders, returns, password-reset records, or guest tracking.

Use sufficiently unguessable public tokens/IDs for guest-accessible private resources.

## Data model acceptance checks

- [ ] Variants can own SKU/stock/price where needed.
- [ ] Orders preserve purchase-time snapshots.
- [ ] Payment and fulfillment are not conflated.
- [ ] Status transitions are explicit.
- [ ] Historical data is not destroyed by catalog edits.
- [ ] Money uses a safe representation and centralized rounding rules.
- [ ] Foreign keys/indexes match real invariants and queries.
- [ ] Guest-accessible private resources use unguessable identifiers/tokens.
