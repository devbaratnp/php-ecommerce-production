# Global Shell and Navigation

## Goals

The shell gives customers persistent orientation and reliable access to product discovery, cart, account, support, and business information.

## Desktop header contract

Common elements:

- brand/home link;
- primary category navigation or clear Shop/Categories entry;
- prominent search;
- account/sign-in;
- cart with synchronized count;
- optional location/store selector when fulfillment depends on it.

Do not overload the header with every content page. Move secondary information to utility navigation/footer.

## Mobile header contract

Keep the first row compact. Prioritize:

- brand;
- search entry or visible search;
- cart;
- menu/categories as appropriate.

If bottom navigation is used, keep a small stable set of primary destinations such as:

```text
Home | Shop | Cart | Account
```

Only include routes that genuinely exist.

Research basis: Baymard mobile research repeatedly emphasizes an obvious path to all categories/shop because mobile users can otherwise lose browsing access.

## Active route

Communicate current location:

- active nav styling;
- `aria-current="page"` where appropriate;
- category/subcategory state;
- breadcrumb on deep pages.

## Breadcrumbs

Use for category/PDP/deep content where hierarchy aids orientation.

Requirements:

- real links for ancestors;
- current page may be non-link/current state;
- labels reflect customer-facing taxonomy, not raw URL fragments;
- structured data may mirror the user path for discoverability.

## Category navigation

Navigation should reflect how customers think about the catalog, not simply the database tree.

Prefer:

- differentiating labels;
- manageable groups;
- useful subcategories;
- clickable parent headings when they represent a browsable scope;
- obvious "All [Category]" routes when submenus show only subdivisions.

Avoid:

- category names that only internal staff understand;
- extremely narrow categories with a handful of products unless useful;
- mega-menu columns grouped arbitrarily;
- parent labels that look clickable but are plain text with no reason.

## Hover menus

Desktop hover-activated navigation should not open/close instantly in a way that flickers as the pointer moves.

Project standard:

- use an intentional delay (roughly a few hundred ms) before opening/switching;
- tolerate diagonal movement into the submenu;
- support focus/keyboard independent of hover;
- keep top-level links clickable where appropriate.

Do not make essential navigation hover-only.

## Sticky header

Use only when it helps access to search/cart/navigation without consuming too much viewport.

Rules:

- reduce height after scrolling if useful;
- never cover anchor targets or focused fields;
- account for `scroll-padding-top`;
- test at 200% zoom/reflow;
- do not stack with multiple other sticky bars until content area becomes tiny.

## Sticky UI budget

Inventory all fixed/sticky elements:

- announcement bar;
- header;
- PDP purchase bar;
- mobile bottom nav;
- chat/WhatsApp button;
- cookie banner;
- toast notifications.

Test combinations, not each component alone.

## Announcement/promo bar

Good for concise real information:

- free-delivery threshold;
- temporary store notice;
- verified promotion.

Do not rotate several critical messages rapidly. Do not create fake urgency.

If closable, remember dismissal appropriately and keep close control accessible.

## Footer

At minimum provide relevant direct paths such as:

- contact/support;
- shipping/delivery information;
- return/refund policy;
- payment information;
- privacy/terms;
- business/company information;
- social links if maintained.

Research basis: Baymard observes a subgroup of users consistently seek shipping and return information directly in the footer, so a generic "Help" link alone is weaker than explicit labels.

## Social/contact links

- `tel:` for phone where appropriate;
- `mailto:` when email is provided;
- WhatsApp link must have context/label;
- external social links should be valid and not placeholder `#`;
- don't open every internal navigation route in a new tab.

## Navigation and JS

Navigation links should remain real `<a href="...">` destinations even when JS enhances transitions/menus.

This helps accessibility, resilience, and search crawling.

## Global shell acceptance checks

- [ ] Logo returns home.
- [ ] Search/cart/account are real destinations.
- [ ] Category browsing is discoverable on mobile and desktop.
- [ ] Current route/scope is understandable.
- [ ] Breadcrumb works on deep routes.
- [ ] Sticky elements do not obscure focus/content.
- [ ] Footer exposes shipping/returns/support explicitly.
- [ ] No dead/hash-only nav links ship.
- [ ] Keyboard users can operate dropdown navigation.
- [ ] Shell works with JS failure for core navigation.
