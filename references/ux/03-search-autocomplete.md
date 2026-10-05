# Search, Autocomplete, and No-Results Recovery

## Goal

Support customers who know an exact product, know only a product type, know desired attributes/use cases, or use imperfect language.

Search is not a SQL `LIKE '%q%'` box with a prettier UI.

## Search intent classes

A capable store may need to handle:

- exact product/model/SKU;
- product type/category;
- brand + product;
- attribute/feature ("256GB phone", "leather bag");
- compatibility ("charger for ...");
- use case/problem ("gift for newborn", "milk for coffee");
- common abbreviation/slang;
- misspelling/spacing;
- non-product support queries if the site search intentionally covers content.

The required depth depends on catalog size and business.

## Search input contract

- visible label or accessible name;
- meaningful placeholder as supplementary hint, not the only label;
- search button or clear Enter behavior;
- clear-input control when useful;
- no submit dead end;
- preserve query in results field;
- reasonable max length and abuse protection.

## Search normalization

Possible normalization:

- trim whitespace;
- normalize repeated spaces;
- case-insensitive comparison according to DB/collation;
- controlled synonym map;
- SKU normalization if business uses punctuation;
- optional typo/fuzzy handling.

Do not aggressively rewrite queries in ways that destroy exact model/SKU searches.

## Search ranking

A simple ranking strategy may prioritize:

1. exact SKU/model;
2. exact/near product name;
3. category/type;
4. brand;
5. attribute matches;
6. description/content.

Catalog size may justify a dedicated search engine. Do not implement fragile hand-written relevance SQL indefinitely if search is core and catalog is large.

## Autocomplete contract

Autocomplete can include:

- query suggestions;
- categories/scopes;
- products;
- recent searches if privacy-appropriate;
- popular searches if genuine.

### Interaction

MUST support:

- Arrow Up/Down navigation when keyboard suggestions exist;
- Enter selection/submission;
- Escape close;
- pointer/touch;
- screen-reader-aware combobox/listbox semantics for custom UI;
- request cancellation/debouncing;
- stale-response protection.

### Length

Keep suggestions scannable. Research from Baymard indicates overly long autocomplete lists add cognitive load; desktop and mobile should show a bounded useful set rather than dozens.

Project default:

- desktop: roughly up to 8–10 visible suggestions;
- mobile: roughly 4–8 depending on viewport/content.

These are defaults, not magic limits.

### Distinguish suggestion types

Make it visually obvious whether a suggestion means:

- search term;
- category scope;
- direct product.

Example:

```text
milk powder
milk powder for babies
milk powder  [in Dairy]
Nangrow 3 Milk Powder  [Product]
```

## Search results page

Should show:

- query;
- result count;
- sort;
- relevant filters;
- product list;
- applied filters;
- corrected/alternative query if used;
- no-results/error state.

Search results need category-specific filters when possible, not one generic filter set for all product types.

## Typo handling

If correcting a query:

```text
Showing results for “samsung”
Search instead for “samsng”
```

or equivalent, especially where exact strings/SKUs can matter.

Do not silently transform every unfamiliar query into a popular one.

## No-results state

A good no-results page acknowledges the failed query and creates recovery paths.

Possible structure:

```text
No results for “iphon 18 blu”

Try:
- iPhone 18
- Blue iPhones
- Apple Phones

[Clear filters]
[Browse Phones]
```

Depending on context add:

- spelling correction;
- related queries;
- parent categories;
- popular/recent items;
- contact/support for specialized products.

Do not show random best sellers above the explanation so the user thinks they are search matches.

## Zero results caused by filters

Distinguish:

```text
No products match all 5 selected filters
```

from:

```text
No products found for the query
```

Offer removal of restrictive filters.

## Search logs and analytics

Track privacy-safe aggregates:

- search query frequency;
- zero-result queries;
- query -> product click rate;
- query -> add-to-cart/purchase where analytics supports it;
- frequently corrected/synonym queries.

Use this data to improve taxonomy and synonym maps.

## Search acceptance checks

- [ ] Exact product/SKU works where expected.
- [ ] Product-type search returns browseable results with relevant filters.
- [ ] Common typo/synonym behavior is defined.
- [ ] Enter always reaches a results page even without selecting autocomplete.
- [ ] Autocomplete is keyboard/touch accessible.
- [ ] Stale async results cannot replace newer query suggestions.
- [ ] No-results page offers real recovery paths.
- [ ] Active filters causing zero results can be cleared.
- [ ] Search results preserve query/filter/sort state across navigation.
