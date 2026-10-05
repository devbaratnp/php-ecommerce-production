# Product Listing Pages, Filters, Sorting, and Product Cards

## PLP job

Help customers scan a set, narrow it, compare enough attributes, and decide which products deserve deeper evaluation.

Research basis: Baymard treats product lists/filtering as a major standalone research domain and reports high abandonment when users lack adequate browsing/filtering support.

## PLP header

Include:

- category/search identity;
- result count;
- optional category description kept concise;
- sort;
- filters;
- applied filter overview when active.

Do not push the first product row far below the fold with oversized banners on every category page.

## Product grid/list

Choose density by vertical.

### Visual products

Fashion, jewelry, furniture may prioritize larger imagery.

### Spec-heavy/B2B

A table/list with comparable columns can outperform image-heavy cards.

Do not use one universal card format for every store archetype.

## Product card contract

Applicable data:

- meaningful product image;
- brand/category if it aids differentiation;
- product name;
- price;
- real sale/compare-at price;
- unit price for grocery/commodity where useful;
- real rating + count when reviews exist;
- stock/availability if decision-relevant;
- color/variant hint where useful;
- delivery/pickup cue if key to comparison;
- clear PDP link.

Product name/image/price must describe the same item/variant scope.

## Secondary imagery

For visually evaluated products, give access to multiple meaningful images without requiring immediate PDP navigation.

Patterns:

- desktop hover/focus secondary image;
- dots/thumbnail strip;
- mobile swipe only if it does not hijack vertical scrolling.

Research basis: Baymard's product-list research supports additional thumbnails/secondary imagery for evaluation-heavy categories.

## Variant representation

Avoid filling a list with nearly identical cards solely because color/size differs.

Prefer one product card with meaningful swatches/count when variants represent the same product concept.

Separate list items only when variants are marketed/evaluated as distinct products.

## Image rules

- consistent aspect ratio within list;
- packaged products often use `object-fit: contain`;
- lifestyle/fashion may use cover where intentional;
- lazy-load below-fold images;
- reserve dimensions;
- meaningful `alt` text for informative images;
- fallback for missing user-uploaded media.

## Filters

Only show filters backed by real data and query logic.

Possible types:

- checkbox multi-select;
- radio/single select;
- numeric/price range;
- color swatch + text;
- hierarchical category;
- availability;
- brand;
- ratings;
- compatibility.

### Filter counts

If counts are shown, they must accurately represent the current narrowing model.

### Multi-select

Within one dimension, multi-select OR behavior is often expected:

```text
Brand = Apple OR Samsung
```

Across dimensions, AND behavior:

```text
(Apple OR Samsung) AND 256GB AND In Stock
```

Document project semantics explicitly.

## Applied filter overview

After applying filters, show visible summary outside the filter drawer/rail:

```text
Apple ×   256GB ×   In Stock ×   Clear all
```

Research basis: current Baymard mobile/PLP research highlights the orientation problem when selected filters are visible only inside the filtering interface.

## Desktop filtering

For catalogs that benefit from filtering:

- persistent rail or clearly visible controls;
- long filter groups may collapse vertically;
- filter search for very long brand/value sets;
- preserve scroll reasonably after changes.

## Mobile filtering

Use a labeled filter/sort button opening an accessible sheet/dialog.

Requirements:

- title;
- close;
- active count;
- selected values retained;
- Apply action if changes are staged;
- Clear controls;
- backdrop/Escape according to dialog pattern;
- focus management;
- no duplicate element IDs from desktop filter copy.

Decide either:

1. apply immediately per selection; or
2. stage choices then Apply.

Do not mix unpredictably.

## Sorting

Provide only sorts customers understand and business can compute correctly.

Typical:

- relevance (search);
- featured/default;
- price low-high / high-low;
- newest;
- rating/popularity only when based on real data.

Changing sort MUST preserve current filters/query and normally reset pagination to first page.

## Pagination/load more/infinite scroll

Default project preference for ordinary PHP stores:

- server pagination as robust base;
- MAY enhance with "Load more" while maintaining URL/history state;
- avoid endless infinite scroll that makes footer inaccessible or destroys return position.

Whichever model:

- do not issue unbounded queries;
- preserve filters/sort;
- expose result progress;
- preserve return context from PDP where feasible.

## Empty/error states

Empty filtered state:

- explain scope;
- show active filters;
- offer clear/remove actions.

Catalog load error:

- acknowledge failure;
- retry;
- preserve query/filter state.

## PLP acceptance checks

- [ ] Result count reflects current scope.
- [ ] Product cards expose enough differentiation for the vertical.
- [ ] Filters are backed by real catalog attributes.
- [ ] Applied filters remain visible outside mobile drawer.
- [ ] Sort preserves filters/query.
- [ ] Filter changes reset page appropriately.
- [ ] Pagination/load more preserves state and avoids unbounded DB work.
- [ ] Returning from PDP restores useful list context.
- [ ] Empty/error states provide recovery.
- [ ] Product imagery and variant identity remain consistent.
