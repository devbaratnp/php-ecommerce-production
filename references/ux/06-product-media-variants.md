# Product Media and Variant Selection

## Product media goals

Images/video must reduce uncertainty, not merely decorate the page.

## Media roles

Depending on product type, provide:

1. primary clean product image;
2. alternate angles;
3. detail/close-up;
4. scale/context image;
5. product in use/on person;
6. package/included contents;
7. variant-specific imagery;
8. video/360 demonstration when it materially helps.

### Vertical examples

Fashion/jewelry:
- on-person/context image is often critical for scale/fit.

Furniture:
- room/context + dimensions/scale.

Electronics:
- ports, included accessories, interface/screens, side/back views.

Grocery:
- packaging front/back, size/weight, nutrition/ingredients if relevant.

## Gallery behavior

Desktop:

- visible main image;
- thumbnails or clearly signposted gallery navigation;
- zoom/fullscreen when fine detail matters;
- keyboard operability.

Mobile:

- natural swipe;
- image count/dots so customers know more images exist;
- pinch/zoom or explicit zoom if needed;
- do not trap vertical page scroll.

## Hidden thumbnails

If not all thumbnails fit, visibly indicate more media exists rather than truncating silently.

## Variant synchronization

Selecting a color/variant should update relevant:

- image gallery;
- SKU;
- price;
- stock;
- availability;
- URL/deep-link state where implemented;
- structured data where page architecture requires it.

Do not show a blue product image while the selected variant says red.

## Variant selector choice

### Small discrete sets

Use visible buttons/swatches:

```text
Size: [S] [M] [L] [XL]
```

Visible controls expose availability better than a hidden dropdown for short sets.

### Long sets

A select, searchable list, matrix, or grouped control may be more practical.

Examples:

- dozens of lengths;
- industrial dimensions;
- many compatible models.

## Unavailable variants

Keep unavailable values visible when doing so helps users understand the product offering.

Pattern:

```text
[S] [M] [L - unavailable] [XL]
```

Do not simply remove unavailable sizes and make customers wonder whether they ever existed.

Use accessible disabled/unavailable semantics and sufficient visual contrast.

## Dependent options

If one choice affects another:

```text
Color -> available sizes
```

then:

- update unavailable states immediately;
- never leave an impossible combination selected;
- explain when a prior selection was cleared.

## Required choice

Do not preselect a variant purely to avoid validation when the selection is meaningful (e.g. size) unless there is a sensible default customers can safely accept.

If user presses Add to Cart without required option:

- keep page context;
- highlight the missing group;
- show inline message;
- move/focus appropriately without disorienting user.

## Quantity

Use a labeled number input/stepper.

- allow direct typing when practical;
- buttons have accessible names;
- disable/deal with min/max accurately;
- server remains authoritative.

## Media performance

- main likely LCP image loads eagerly/high priority;
- thumbnails/secondary media can lazy-load;
- use responsive `srcset`/sizes or equivalent;
- reserve dimensions/aspect ratio;
- upload pipeline should generate appropriately sized derivatives.

See `12-performance.md`.

## Media accessibility

Alt text should describe information the image conveys, not stuff keywords.

Decorative thumbnails can avoid redundant announcements while controls still need accessible labels like:

```text
View image 3: back of black backpack
```

Video needs captions/transcript where content requires it and controls must be keyboard usable.

## Acceptance checks

- [ ] More media is visibly signposted.
- [ ] Swipe/zoom/keyboard behavior works by viewport.
- [ ] Selected variant and media stay synchronized.
- [ ] Unavailable variants are understandable.
- [ ] Required option errors are inline and recoverable.
- [ ] Quantity control is keyboard/touch accessible.
- [ ] Main image is not incorrectly lazy-loaded.
- [ ] Variant URLs/deep links reproduce the correct product state when supported.
