# Performance and Core Web Vitals

## Principle

Performance is part of ecommerce UX. Slow product discovery, laggy filters, delayed purchase feedback, and layout shift reduce trust and task completion.

## Release performance targets

When field data is available, aim for "good" Core Web Vitals at the 75th percentile:

- **LCP <= 2.5 seconds**
- **INP <= 200 ms**
- **CLS <= 0.1**

These targets follow current web.dev/Core Web Vitals guidance as of this skill's research refresh.

Lab tools help diagnose but do not replace real-user data.

## LCP priorities

Likely LCP on ecommerce pages:

- homepage hero/category image;
- first PLP product/heading block;
- PDP primary product image.

MUST:

- make likely LCP resource discoverable early;
- do not `loading="lazy"` the main above-the-fold LCP image;
- use appropriate preload/fetch priority only where it actually helps;
- compress and right-size images;
- avoid delaying primary content behind JS rendering.

## Responsive images

Use `srcset`/`sizes` or a capable image service to avoid sending desktop-resolution files to small phones.

Store/generated derivatives should cover common display widths.

Modern formats may be used with suitable fallback/processing.

## Layout stability

Reserve dimensions for:

- product images;
- banners;
- review widgets;
- payment widgets;
- recommendations;
- async stock/delivery blocks.

Do not insert announcement bars above content after load without reserving space.

Use stable skeletons only when they preserve expected geometry.

## Interaction responsiveness

INP-sensitive components include:

- search autocomplete;
- filter application;
- variant selection;
- quantity controls;
- add to cart;
- cart drawer;
- checkout validation.

Avoid long main-thread JS tasks for behaviors PHP/HTML can handle simply.

## JavaScript budget

Project principle:

- progressive enhancement;
- load only required libraries;
- defer noncritical third-party widgets;
- do not load an animation framework for a few fades;
- avoid duplicate libraries;
- split admin/storefront scripts if they serve different audiences.

Third-party chat, analytics, heatmaps, reviews, ads, and payment scripts can dominate performance. Audit them individually.

## CSS

- avoid giant framework bundles when only a small subset is needed;
- critical layout should not wait for unnecessary styles;
- remove obsolete legacy CSS/animation effects;
- avoid expensive layout-triggering animations; prefer transform/opacity.

## Product lists

- paginate server-side;
- do not return thousands of products in one query/render;
- lazy-load below-fold images;
- use thumbnail-sized image derivatives;
- filter/sort queries must be indexed/tuned;
- cache stable category/facet data when safe.

## Search/autocomplete

- debounce network calls reasonably;
- abort/ignore stale requests;
- cap suggestions/results;
- index searchable DB columns or use dedicated search tech when catalog scale demands it;
- do not run expensive wildcard queries across huge unindexed blobs on every keypress.

## Database

Measure slow queries.

Common optimizations:

- index category/status/price/search keys based on real queries;
- avoid N+1 product/variant/media queries;
- fetch only required columns;
- bounded pagination;
- aggregate stats asynchronously/cached where feasible;
- explain/query plan before guessing.

## Caching

Candidates:

- static assets via long cache headers + fingerprints;
- category/navigation structure;
- product pages with careful invalidation for price/stock fragments;
- computed recommendations;
- admin report aggregates.

Never cache private account/admin HTML publicly.

## Fonts

- minimize families/weights;
- self-host or load efficiently when appropriate/licensed;
- `font-display` strategy prevents invisible text;
- do not ship multi-megabyte font sets for minor branding differences.

## Performance budgets

For a project, set page-specific budgets such as:

```text
PDP initial image bytes: ...
Initial JS compressed: ...
Third-party script count: ...
PLP products per server page: ...
```

Budgets should be realistic for the site's traffic/device/network environment.

## Measurement

Use:

- browser Performance panel;
- Lighthouse/PageSpeed as diagnostics;
- WebPageTest where useful;
- server logs/APM/query logs;
- RUM/web-vitals collection after launch.

Record page type, device class, and release version so regressions are traceable.

## Performance acceptance checks

- [ ] Likely LCP image is not lazy-loaded.
- [ ] Product/list images are responsive and right-sized.
- [ ] Image/layout dimensions prevent avoidable CLS.
- [ ] Search/filter/cart interactions avoid long blocking work.
- [ ] Product queries are bounded and indexed appropriately.
- [ ] No obvious N+1 queries on catalog/admin tables.
- [ ] Third-party scripts are justified and deferred where possible.
- [ ] Core Web Vitals measured in lab and field when possible.
