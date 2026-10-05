# Catalog and Inventory

## Purpose

Define how products become discoverable, purchasable units and how stock remains consistent across storefront and operations.

## Product lifecycle

Typical product states:

- draft;
- active/published;
- archived;
- optionally scheduled.

Do not treat deletion as the default lifecycle operation when historical orders reference the product. Archive/disable is usually safer.

## Product vs variant

A product is the conceptual item. A variant is the purchasable configuration.

Example:

```text
Product: Classic T-Shirt
Variants:
- Black / S / SKU TS-BLK-S
- Black / M / SKU TS-BLK-M
- White / M / SKU TS-WHT-M
```

The storefront may show one product card while cart/inventory operate on variant IDs.

## Attributes

Separate three ideas:

1. **variant-defining options**: color, size, weight;
2. **filter/specification attributes**: material, capacity, compatibility;
3. **content fields**: descriptive prose.

Do not encode every attribute in freeform text if users need to filter/compare it.

## Catalog integrity

MUST ensure:

- active product has valid purchasable state or communicates non-purchasable state;
- displayed variant belongs to the current product;
- variant SKU is unique according to business rules;
- product media and option values map consistently;
- categories/filters only expose values backed by real catalog data;
- prices/stock in cards and PDP derive from the same current source.

## Inventory policy

Explicitly decide:

- sell only on-hand stock;
- reserve stock at checkout start;
- reserve only after order creation;
- allow backorders/preorders;
- oversell threshold;
- multi-location allocation.

Do not let different endpoints assume different policies.

## Simple stock model

For ordinary stores, a central available calculation can be:

```text
available = on_hand - reserved
```

If reservations are not used:

```text
available = on_hand
```

Do not show inventory counts more precise than the business wants customers to see.

## Concurrency

Two customers may attempt to buy the last unit simultaneously.

The checkout service must enforce stock within a transaction/locking strategy appropriate to the schema, for example:

1. begin transaction;
2. read/lock relevant inventory row;
3. verify requested quantity against current available quantity;
4. decrement/reserve;
5. create order/order items;
6. commit.

A prior AJAX "stock available" check is informative only; it does not protect the final order.

## Stock movements

For businesses needing traceability, keep an inventory movement ledger:

```text
purchase/initial stock +10
sale/order          -2
return/restock       +1
manual adjustment    -1
transfer out          -5
transfer in           +5
```

Each movement should record:

- variant;
- quantity delta;
- reason/type;
- source record/order if relevant;
- actor;
- timestamp;
- optional note.

Do not allow operators to silently overwrite stock without an audit trail when stock accuracy matters.

## Multi-location stock

When activated:

- each location has its own inventory level;
- storefront availability may aggregate or select a fulfillment location;
- admin can transfer stock explicitly;
- order allocation records the source location;
- negative inventory policy is per business rule.

Do not expose location-level complexity to customers unless it helps fulfillment/pickup decisions.

## Out-of-stock UX data

Backend should expose enough state for UX to distinguish:

- in stock;
- low stock if business chooses;
- out of stock;
- temporarily unavailable;
- preorder/backorder;
- discontinued.

"Out of stock" should not be overloaded to mean every non-purchasable situation.

## Catalog imports

For bulk product imports:

1. parse into staging representation;
2. validate SKU/category/price/attribute values;
3. show row errors;
4. preview create/update counts;
5. apply in batches/transactions;
6. log import summary.

Define whether SKU is the stable matching key for updates.

## Product deletion

Prefer:

- archive product;
- prevent new sales;
- keep historical order snapshots;
- keep old product route behavior intentional (404, archived page, substitute redirect depending on SEO/business need).

## Inventory acceptance tests

- [ ] Two concurrent checkout attempts cannot both consume unavailable stock.
- [ ] Variant stock is independent where variants exist.
- [ ] Manual stock adjustments are auditable where required.
- [ ] Archived products cannot be newly purchased.
- [ ] Cart/checkout revalidate stock.
- [ ] Returns only restock according to configured condition/rules.
- [ ] Multi-location allocation is deterministic if enabled.
- [ ] Product/variant/media mappings remain consistent.
