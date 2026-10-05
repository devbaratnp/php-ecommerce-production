---
name: php-ecommerce
description: Use when building or auditing a PHP/MySQL ecommerce site — covers session auth, CSRF, SQL injection prevention, cart/checkout, admin CRUD, security headers, and role-based access
---

# PHP Ecommerce Patterns

## Overview

Proven patterns from hardening a production PHP/MySQL ecommerce site. Covers security, cart/checkout flow, admin panel CRUD, and role-based access.

---

## 1. Security Hardening

### Authentication & Session

```php
// config.php — always require this first
session_set_cookie_params(['httponly' => true, 'samesite' => 'Strict']);
session_start();

// Regenerate after login
session_regenerate_id(true);

// Session timeout
if (isset($_SESSION['last_activity']) && (time() - $_SESSION['last_activity'] > 7200)) {
    session_destroy();
    header('Location: ' . BASE_URL . '/login.php');
    exit;
}
$_SESSION['last_activity'] = time();
```

**DO:**
- Call `session_regenerate_id(true)` after login/register
- Set session timeout with configurable TTL
- Use HttpOnly + SameSite cookies
- Set `session.use_strict_mode = 1`
- Destroy + redirect on timeout expiry

**DON'T:**
- Store raw passwords — always `password_hash()` / `password_verify()`
- Expose session data in error messages
- Skip timeout on admin pages

### CSRF Protection

```php
// Generate token in config.php
if (empty($_SESSION['csrf_token'])) {
    $_SESSION['csrf_token'] = bin2hex(random_bytes(32));
}

// Include in every form
<input type="hidden" name="csrf_token" value="<?= $_SESSION['csrf_token'] ?>">

// Validate on POST handlers
if (!hash_equals($_SESSION['csrf_token'], $_POST['csrf_token'])) {
    die('Invalid CSRF token');
}
```

**DO:** Validate CSRF on ALL POST handlers — login, register, contact, checkout, booking, newsletter, cart-update, admin CRUD.

### SQL Injection Prevention

```php
// GOOD — Prepared statements
$stmt = $db->prepare("SELECT * FROM users WHERE email = ?");
$stmt->bind_param('s', $email);
$stmt->execute();
$result = $stmt->get_result();

// GOOD — Integer casting for IDs
$id = (int)$_GET['id'];

```php
// BAD — String interpolation
$result = $db->query("SELECT * FROM users WHERE email = '$email'");
```

**DO:** Prepared statements (`bind_param`) for all user input. Use `intval` / `(int)` cast for IDs in inline queries. Error details go to server logs only (`error_log()`), never to output.

### XSS Prevention

```php
// ALWAYS escape output
<?= htmlspecialchars($user['name'], ENT_QUOTES, 'UTF-8') ?>
```

**DO:** Wrap ALL user/database output in `htmlspecialchars()`. Even in admin panels.

**DON'T:** Use `<?= $var ?>` directly unless `$var` is hardcoded or a known-safe constant.

### Security Headers

```php
header('X-Content-Type-Options: nosniff');
header('X-Frame-Options: DENY');
header('Referrer-Policy: strict-origin-when-cross-origin');
header("Content-Security-Policy: default-src 'self' https:; style-src 'unsafe-inline' https:; script-src 'unsafe-inline' https:;");
```

### Directory Protection (.htaccess)

```apache
Options -Indexes
RewriteRule ^\.git - [F,L]
```

Placeholder `index.php` files in all empty directories with `header('Location: ...'); exit;`.

---

## 2. Role-Based Auth

### Admin Guard

```php
// Top of every admin page
require_once __DIR__ . '/../includes/config.php';
if (!isset($_SESSION['user_id']) || ($_SESSION['role'] ?? '') !== 'admin') {
    header('Location: ' . BASE_URL . '/login.php');
    exit;
}
```

### Login Redirect

```php
if ($user['role'] === 'admin') {
    $redirect = BASE_URL . '/admin/dashboard.php';
} else {
    $redirect = $_SESSION['redirect_after_login'] ?? BASE_URL . '/index.php';
}
unset($_SESSION['redirect_after_login']);
header('Location: ' . $redirect);
exit;
```

---

## 3. Admin Panel Architecture

### Directory Structure

```
admin/
├── includes/
│   ├── head.php        # <head> with Tailwind config, CDN links
│   └── sidebar.php     # Nav sidebar with $active_page highlighting
├── dashboard.php
├── orders.php
├── products.php
├── categories.php
├── doctors.php
├── appointments.php
├── blog.php
└── users.php
```

### Admin Sidebar Pattern

```php
// sidebar.php — dynamic nav with active state
$nav_items = [
    'dashboard'    => ['label' => 'Dashboard',   'icon' => 'dashboard'],
    'orders'       => ['label' => 'Orders',       'icon' => 'shopping_bag'],
    'products'     => ['label' => 'Products',     'icon' => 'medication'],
    'categories'   => ['label' => 'Categories',   'icon' => 'category'],
    'doctors'      => ['label' => 'Doctors',      'icon' => 'stethoscope'],
    'appointments' => ['label' => 'Appointments', 'icon' => 'calendar_month'],
    'blog'         => ['label' => 'Blog Posts',   'icon' => 'article'],
    'users'        => ['label' => 'Users',        'icon' => 'group'],
];
foreach ($nav_items as $key => $item):
    $is_active = ($active_page === $key);
?>
<a class="<?= $is_active ? 'bg-secondary-container...' : 'text-on-surface-variant...' ?>" href="<?= $key ?>.php">
```

### Admin CRUD Pattern

```php
// At TOP of file (before HTML output):
// 1. Handle POST (create/update/delete)
if ($_SERVER['REQUEST_METHOD'] === 'POST' && isset($_POST['action'], $_POST['csrf_token'])
    && hash_equals($_SESSION['csrf_token'], $_POST['csrf_token'])) {

    if ($_POST['action'] === 'create') {
        $stmt = $db->prepare("INSERT INTO table (cols) VALUES (?, ?)");
        $stmt->bind_param('ss', $_POST['field1'], $_POST['field2']);
        $stmt->execute();
        header('Location: page.php');
        exit;
    }
    if ($_POST['action'] === 'update') {
        // UPDATE ...
    }
    if ($_POST['action'] === 'delete') {
        $stmt = $db->prepare("DELETE FROM table WHERE id = ?");
        $stmt->bind_param('i', $_POST['id']);
        $stmt->execute();
        header('Location: page.php');
        exit;
    }
}

// 2. Fetch data for list
$search = trim($_GET['search'] ?? '');
$page = max(1, (int)($_GET['page'] ?? 1));
$per_page = 20;
$offset = ($page - 1) * $per_page;

// Build dynamic WHERE for search/filter
$where = [];
$params = [];
if ($search) {
    $where[] = "name LIKE ?";
    $params[] = "%$search%";
}
// ... execute with pagination

// 3. Render HTML using shared includes
require_once __DIR__ . '/includes/head.php';
// body + sidebar
// table with edit/delete forms per row
```

### Status Update Form (Inline Table)

```php
<form method="POST" class="flex items-center gap-2" onsubmit="return confirm('Update status?')">
    <input type="hidden" name="csrf_token" value="<?= $_SESSION['csrf_token'] ?>">
    <input type="hidden" name="id" value="<?= $row['id'] ?>">
    <select name="status">
        <option value="pending" <?= $row['status']==='pending' ? 'selected' : '' ?>>Pending</option>
        <option value="confirmed" ...>Confirmed</option>
    </select>
    <button type="submit" name="update_status" value="1">Update</button>
</form>
```

### Pagination

```php
<div class="flex gap-2">
    <?php if ($page > 1): ?>
        <a href="?page=<?= $page-1 ?>&search=<?= urlencode($search) ?>">Previous</a>
    <?php endif; ?>
    <span>Page <?= $page ?> of <?= $total_pages ?></span>
    <?php if ($page < $total_pages): ?>
        <a href="?page=<?= $page+1 ?>&search=<?= urlencode($search) ?>">Next</a>
    <?php endif; ?>
</div>
```

**DO:**
- Preserve search/filter params across pagination
- Clear filters link when search/status active
- Show "X of Y" at bottom
- Stats cards show grand totals (unfiltered), pagination shows filtered counts

---

## 4. Ecommerce Cart/Checkout

### Cart Storage Strategy

| User State | Storage | Key |
|---|---|---|
| Logged in | `cart` DB table | `user_id` |
| Guest | `$_SESSION['cart']` array | Session |

### Cart CRUD Handler

```php
// cart-update.php
if (!isset($_SESSION['user_id'])) {
    // Session-based cart
    if ($action === 'add') {
        $_SESSION['cart'][$product_id] = ($_SESSION['cart'][$product_id] ?? 0) + 1;
    }
} else {
    // DB cart
    $stmt = $db->prepare("INSERT INTO cart (user_id, product_id, quantity) VALUES (?, ?, 1)
                          ON DUPLICATE KEY UPDATE quantity = quantity + 1");
    $stmt->bind_param('ii', $_SESSION['user_id'], $product_id);
    $stmt->execute();
}
// Always sync badge count
$_SESSION['cart_count'] = array_sum($_SESSION['cart'] ?? []);
```

### Checkout Flow (PRG Pattern)

```php
// checkout.php
if ($_SERVER['REQUEST_METHOD'] === 'POST') {
    // Validate stock, save order, clear cart
    $_SESSION['order_success'] = ['order_number' => $order_number];
    header('Location: checkout.php?order=success');
    exit;
}
if (isset($_GET['order']) && $_GET['order'] === 'success') {
    $order = $_SESSION['order_success'];
    unset($_SESSION['order_success']);
    // Show success message
}
```

**DO:** PRG (Post-Redirect-Get) pattern to prevent duplicate submissions. Validate stock before placing order. Save shipping address in orders table. Require login before checkout (redirect with `redirect_after_login`).

---

## 5. Audit Checklist

### Security
- [ ] `session_regenerate_id(true)` after login/register
- [ ] Session timeout implemented
- [ ] HttpOnly + SameSite cookies
- [ ] CSRF token on every form
- [ ] All POST handlers validate CSRF with `hash_equals()`
- [ ] All queries use prepared statements
- [ ] All output wrapped in `htmlspecialchars()`
- [ ] Error details logged, never exposed to user
- [ ] Security headers set (`X-Content-Type-Options`, `X-Frame-Options`, `Referrer-Policy`)
- [ ] `.htaccess` blocks directory listing + `.git` access
- [ ] Empty directories have `index.php` placeholders

### Auth
- [ ] Role-based redirect after login (admin → dashboard, customer → intended page)
- [ ] Admin guard on every admin page
- [ ] Password hashing with `password_hash()`
- [ ] `BASE_URL` constant for all redirects

### Ecommerce
- [ ] Cart works for both logged-in (DB) and guest (session) users
- [ ] Stock validation before order placement
- [ ] PRG pattern on checkout
- [ ] Shipping address saved to orders table
- [ ] Cart badge count syncs after every update

### Admin
- [ ] Shared components for head/sidebar (DRY)
- [ ] Functional search + status filter
- [ ] Functional pagination with params preserved
- [ ] Status update forms with CSRF
- [ ] Stats cards show unfiltered totals
- [ ] `LIMIT ? OFFSET ?` on all queries (no unbounded queries)
- [ ] Every admin page has auth guard

---

## 6. Common Mistakes

| Mistake | Fix |
|---|---|
| `session_start()` before constants defined | Define constants first, then call `session_start()` |
| Admin page uses public `header.php` | Use standalone admin includes with sidebar |
| Hardcoded BASE_URL or redirect paths | Use `BASE_URL` constant everywhere |
| Stats use filtered count | Use `$grand_total` for stats, `$filtered_total` for pagination |
| Missing `exit;` after `header('Location: ...')` | Always `header() + exit` |
| `mysqli->query()` with user input | Always `prepare()` + `bind_param()` |
| Cart badge not updated on AJAX add | Sync `$_SESSION['cart_count']` in every cart mutation |
| Password toggle button has no JS handler | Add `onclick` handler that toggles `input.type` |

---

## 7. Key File Responsibilities

| File | Purpose |
|---|---|
| `includes/config.php` | Session boot, constants (BASE_URL, SITE_NAME), CSRF token, security headers, error suppression |
| `config/database.php` | MySQL connection via `getDB()`, error logging |
| `cart-update.php` | Cart CRUD for logged-in (DB) + guest (session) |
| `checkout.php` | Login guard, stock check, order save, PRG redirect |
| `admin/includes/sidebar.php` | Dynamic nav with `$active_page` |
| `login.php` | Auth with `session_regenerate_id`, role-based redirect |
| `.htaccess` | `Options -Indexes`, `.git` block |

---

## 8. PHP Version Notes

- Written for PHP 8.x
- Use `match()` instead of `switch` where appropriate
- Use `str_contains()` instead of `strpos() !== false`
- Use `readonly` properties where applicable (PHP 8.1+)
- Use `enum` for status values if PHP 8.1+ (otherwise DB ENUM)

---

## 9. Ecommerce UX Pattern Library

Use this section when building or auditing the customer-facing storefront. Treat each pattern as a behavior contract, not decoration: every visible control must either work, communicate its state, or be removed.

### Global storefront shell

- Use a persistent desktop header with clear brand, search, primary navigation, account, and cart entry points.
- On mobile, keep the header compact and provide persistent bottom navigation for Home, Shop, Cart/Bag, and Account.
- Give the current route an obvious active state and expose aria-current="page".
- Make cart and account links real destinations that still work when JavaScript is disabled.
- Add a skip link, semantic landmarks (header, nav, main, footer), a logical heading order, visible keyboard focus, and touch targets of at least 44px.
- Offset content for fixed headers and bottom navigation so titles, dialogs, and focused fields are never covered.
- Use one consistent z-index scale; dialogs and sheets must sit above the shell without trapping unrelated content.

### Discovery, search, and navigation

- Support category browsing without requiring a keyword.
- Preserve search, category, sort, availability, price, and sale parameters when the user paginates or changes one control.
- Provide search suggestions or recent terms when available; pressing Enter must reach a full results page.
- Show a clear empty-search state with the query, the reason no results were found, and one-click recovery paths.
- Keep breadcrumbs or an equivalent back path on product detail and deep content pages.
- Never ship dead hash links, decorative buttons that imply behavior, or nav items that point to the wrong route.

### Product listing pages

- Show a clear result count, stable product grid, and a sort control with a labeled default.
- On desktop, use a visible filter rail that remains available while scanning the grid.
- On mobile, use a real filter/sort button that opens a bottom sheet or dialog with a clear title and close action, unique labeled controls, an Apply action, active filter count, and dismissal by close button, Escape, and backdrop where appropriate.
- Render active filters as removable chips tied to URL/query state, plus a Clear all action.
- Use only filters backed by real data and query logic. Do not show fictional attributes as if they were interactive.
- Preserve filters when changing sort, category, or page; reset page to 1 when a filter changes.
- Product cards should expose image, category or type, name, price, compare-at price when relevant, stock state, and a clear product/detail action.
- Use a consistent image aspect ratio and object-contain for packaged products. A missing image must use a neutral branded placeholder, never an unrelated stock photo.
- Lazy-load below-the-fold images, include meaningful alt text, and provide an onerror fallback for user-uploaded media.
- Provide a real empty result state with recovery actions and a real error/retry state when catalog loading fails.

### Product detail pages

- Use breadcrumbs, an image gallery with a predictable main image, product name, category, SKU or traceability identifier, price, sale comparison, and stock status.
- Expose variants with labels and selected state; do not hide required choices in unlabelled controls.
- Provide a labeled quantity stepper, Add to cart, and a secondary save/wishlist action only when that action is implemented.
- Make the purchase action remain easy to reach on mobile, either through natural layout or a sticky action bar.
- Explain delivery, returns/replacement, payment methods, provenance, and support near the purchase decision.
- Keep related products relevant to the current category or product, not merely the next database rows.
- Validate image/data consistency: the product image, title, price, description, and variants must describe the same item.

### Cart and checkout

- Show a mini-cart or cart badge that stays synchronized after every add, update, or remove.
- Provide a full cart fallback with product identity, quantity controls, remove action, price calculation, shipping/tax messaging, and a prominent checkout action.
- Design both populated and empty cart states; the empty state should link back to shopping.
- Prevent double-submit with PRG and/or an idempotency guard. Recheck stock and price on the server.
- Use a checkout step indicator when the flow has multiple screens.
- Group shipping/contact/payment fields logically, label every field, preserve valid input after errors, and place errors next to the affected field.
- Before final submission, show an order summary, delivery destination, total, payment method, and the final action’s meaning.
- After success, show a confirmation number, purchased items, next steps, support route, and order history link.

### Account and order history

- Keep account entry discoverable from the header and mobile navigation.
- Support intended-destination return after login.
- Give customers order history with order number, date, amount, status, detail/reorder path, and support contact.
- Provide clear session states: signed out, signed in, expired session, invalid credentials, and forgotten password.
- Never make sign-in the only way to recover a guest cart unless that limitation is explained before the redirect.

### Trust, content, and reassurance

- Surface delivery coverage, shipping threshold, payment methods, returns/replacement, contact methods, and legal links in the footer or purchase context.
- Use real product names, measurements, prices, stock counts, dates, and descriptions. Remove encoding artifacts and placeholder copy before release.
- Explain origin, certifications, or quality claims with evidence and avoid unverified badges.
- Match promotional copy to actual product state: sale labels, stock badges, and pre-order must come from the same server data used by the action.
- Keep social links, phone links, email links, and WhatsApp links labeled and valid.

### State and feedback patterns

- Cover loading/skeleton, success, empty, error/retry, validation, disabled, out-of-stock, and pre-order states.
- Confirm Add to cart with an in-context message, updated badge, or mini-cart; do not rely on a silent page change.
- Use inline validation and readable error text instead of alert() for ordinary form errors.
- Preserve user context after recovery: return to the same query, category, product, or checkout step where safe.
- Respect prefers-reduced-motion; animate opacity/transform rather than layout properties.

### Responsive and accessibility acceptance checks

- Test mobile, tablet, desktop, zoom/reflow, keyboard-only navigation, and a screen-reader-oriented DOM snapshot.
- Ensure fixed navigation, dialogs, sheets, sticky rails, and focus rings behave at narrow widths.
- Use unique id values and matching labels; never duplicate filter IDs between desktop and mobile variants.
- Ensure contrast for body text, disabled states, badges, placeholders, and focus indicators.
- Keep headings hierarchical, buttons descriptive, links destination-oriented, and images accurately described.

### Ecommerce UX audit checklist

- [ ] Header, mobile nav, search, account, and cart have valid destinations
- [ ] Active route and cart count communicate current state
- [ ] Search works for keyword and category-only browsing
- [ ] Listing has count, sort, real filters, active chips, clear all, pagination, and empty/error states
- [ ] Mobile filters open a labeled sheet/dialog and apply query state
- [ ] Product cards have consistent imagery, price, stock, identity, and detail path
- [ ] Product detail has gallery, variants, quantity, purchase action, trust content, and relevant recommendations
- [ ] Cart supports add/update/remove, empty state, totals, and checkout entry
- [ ] Checkout validates input, rechecks stock/price, prevents duplicate orders, and confirms success
- [ ] Account and order history have signed-in/signed-out/error states
- [ ] Accessibility, responsive, and reduced-motion checks pass for the main purchase path
