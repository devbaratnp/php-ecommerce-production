# Wishlist, Save for Later, and Product Comparison

These are optional modules. Do not show their controls until persistence, retrieval, and edge states are implemented.

## Wishlist / saved products

### Use when

Useful for:

- high-consideration purchases;
- fashion/jewelry/furniture;
- products customers revisit;
- cross-device returning customers.

Less important for some immediate-purchase commodity flows.

### Guest behavior

Options:

- session/local browser saved list;
- persistent anonymous server list.

If the guest later logs in, merge idempotently.

Do not force account creation at the moment of saving unless the business intentionally requires cross-device persistence and explains why.

### Saved item state

Store product/variant identity according to UX.

When rendering later:

- current price;
- current availability;
- variant validity;
- changed/discontinued state.

Never show stale saved price as a purchase guarantee unless business explicitly honors it.

### Wishlist actions

- add/remove;
- move/add to cart;
- choose missing variant if needed;
- empty state;
- share only if deliberately implemented with privacy controls.

## Save for later from cart

Useful if customer wants to reduce current cart without losing interest.

Action should:

1. remove from active cart;
2. add to saved list atomically/idempotently where possible;
3. update totals/count;
4. confirm result.

## Compare products

### Activate when

Strong candidate for spec-driven products:

- electronics;
- appliances;
- B2B equipment;
- plans/products with stable comparable attributes.

Less useful for products primarily judged by taste/style.

### Compare selection

- visible checkbox/action on PLP/PDP;
- cap comparison count to a usable number;
- persistent compare tray;
- do not allow fundamentally incompatible categories unless comparison still makes sense.

### Comparison table

Rows should represent meaningful, normalized attributes.

```text
                 Product A     Product B
Price            ...           ...
Capacity         ...           ...
Warranty         ...           ...
Weight           ...           ...
```

Highlight differences optionally, but keep accessibility/contrast.

Do not populate comparison from inconsistent freeform descriptions.

### Mobile

Wide comparison tables are challenging. Consider:

- 2-product compare at a time;
- sticky product headers;
- horizontal table with clear spatial relationship only if necessary;
- "show differences only";
- stacked attribute groups.

## Acceptance checks

- [ ] Wishlist/compare controls only appear when feature is functional.
- [ ] Guest state merges safely on login where supported.
- [ ] Current price/stock shown on saved items.
- [ ] Discontinued/missing variants are explained.
- [ ] Comparison attributes are normalized and meaningful.
- [ ] Mobile comparison remains understandable.
