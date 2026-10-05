# Ecommerce SEO and Discoverability

## Scope

This reference focuses on technical discoverability that overlaps product UX and architecture. It does not promise rankings and should be checked against current Google Search documentation before major implementation changes.

## Crawlable navigation

Public products intended for indexing should be reachable through real links:

```text
Home -> Category -> Subcategory/PLP -> Product
```

Do not rely only on a search form or JavaScript click handlers for discovery.

Google Search Central explicitly recommends crawlable `<a href>` navigation and meaningful ecommerce site structure.

## URLs

Use stable, readable product/category URLs.

Examples:

```text
/products/classic-tshirt
/category/dairy
```

Do not expose implementation filenames/IDs unnecessarily when routes can be stable.

## Canonical strategy

Every indexable canonical page should have a deliberate canonical URL.

Common duplicates:

- sort parameters;
- tracking parameters;
- filter combinations;
- duplicate product routes;
- variant query parameters.

Do not blindly canonicalize every filtered page without understanding search/discovery needs.

## Product variants

Decide:

1. one canonical product page with variant query/state; or
2. separate indexable variant URLs when variants merit independent pages.

Google documents ProductGroup/Product variant structured data and recommends each variant be directly preselectable through a distinct URL when using variant markup patterns.

The chosen URL must reproduce the correct image, price, availability, and selection.

## Product structured data

For purchasable product pages, consider JSON-LD `Product`/`Offer` merchant listing data consistent with visible page data.

Possible properties:

- name;
- image;
- description;
- SKU/GTIN/brand;
- price/currency;
- availability;
- shipping/return information where supported;
- real aggregate rating/reviews when eligible.

Never put fake ratings/prices in structured data that are not visible/real.

Validate with current Google rich result tools/docs.

## Breadcrumb structured data

BreadcrumbList can reflect the normal customer hierarchy.

Structured breadcrumb should agree with visible breadcrumb/navigation, not invent a hierarchy only for search engines.

## Metadata

Product/category pages should have useful:

- `<title>`;
- meta description where managed;
- canonical;
- Open Graph/social metadata if required.

Avoid duplicate boilerplate titles across hundreds of products.

## Faceted navigation

Filters can generate combinatorial URLs.

Define:

- which filter URLs are crawlable/indexable;
- canonical behavior;
- robots/noindex strategy where appropriate;
- internal link behavior;
- sitemap inclusion.

Do not allow every arbitrary sort/filter combination to explode crawl space.

## Pagination / load more

Even if UX uses Load More or infinite enhancement, products intended for indexing should have crawlable paginated URLs/links or another search-compatible architecture.

Preserve real anchors/URLs.

## Out-of-stock/discontinued

Temporary out-of-stock product:

- often keep page available with clear availability and alternatives.

Permanently discontinued:

- decide whether useful content/replacements justify keeping page;
- otherwise use appropriate status/redirect strategy.

Do not redirect every discontinued product to home.

## Empty category

If category has no useful content/products and is permanently gone, choose status/index strategy intentionally. Google documents noindex/404 considerations for empty ecommerce pages.

## Images

- descriptive filenames where practical;
- accurate alt text;
- high-quality product imagery;
- responsive delivery;
- crawlable image URLs as applicable.

SEO never justifies loading enormous images that destroy performance.

## Sitemap

Include canonical URLs intended for indexing. Keep updated after product/category lifecycle changes.

Do not include admin/account/cart/checkout/private pages.

## Robots/staging

Release checklist MUST verify production is not accidentally blocked by:

- staging `robots.txt`;
- global `noindex`;
- auth wall;
- wrong canonical domain.

## Content integrity

Product structured data, visible content, checkout pricing, and inventory should agree.

Search optimization must not create misleading prices/availability.

## Acceptance checks

- [ ] Public products reachable by crawlable links.
- [ ] Canonical URLs are intentional.
- [ ] Variant URL/structured-data strategy documented.
- [ ] Product structured data mirrors real visible data.
- [ ] Faceted navigation crawl strategy defined.
- [ ] Load-more/pagination does not hide products from crawlers.
- [ ] Sitemap contains canonical public catalog pages only.
- [ ] Production robots/noindex/canonical domain verified.
