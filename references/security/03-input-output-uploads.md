# Input, Output, CSRF, SQL, and Upload Safety

## Treat all external input as untrusted

Sources include:

- GET/query strings;
- POST/form data;
- JSON bodies;
- cookies;
- headers;
- file uploads;
- webhook payloads;
- data imported from CSV/Excel;
- admin-entered rich content;
- third-party APIs.

"Admin entered it" does not automatically make data safe for output contexts.

## Validation vs sanitization

Prefer validation against the domain:

- quantity must be an integer within allowed range;
- email must be a syntactically valid email and normalized consistently;
- status must be one of allowed transitions;
- sort option must map to a known value;
- product ID must identify an accessible/published product for storefront operations.

Do not rely on "strip dangerous characters" as a general security strategy.

## CSRF helper contract

Example:

```php
function csrfToken(): string {
    if (empty($_SESSION['csrf_token'])) {
        $_SESSION['csrf_token'] = bin2hex(random_bytes(32));
    }
    return $_SESSION['csrf_token'];
}

function verifyCsrf(string $token): void {
    $known = $_SESSION['csrf_token'] ?? '';
    if ($known === '' || !hash_equals($known, $token)) {
        http_response_code(419);
        exit('Your session expired. Please refresh and try again.');
    }
}
```

Do not call `hash_equals()` with undefined values without handling them.

AJAX/fetch requests can send the token in a header or request body. The server still validates it.

## SQL patterns with PDO

Single value:

```php
$stmt = $pdo->prepare('SELECT * FROM products WHERE id = :id');
$stmt->execute(['id' => $id]);
```

LIKE search:

```php
$stmt = $pdo->prepare('SELECT * FROM products WHERE name LIKE :q LIMIT :limit OFFSET :offset');
$stmt->bindValue(':q', '%' . $query . '%', PDO::PARAM_STR);
$stmt->bindValue(':limit', $limit, PDO::PARAM_INT);
$stmt->bindValue(':offset', $offset, PDO::PARAM_INT);
$stmt->execute();
```

Dynamic filters should construct only known SQL fragments while binding values.

## Output contexts

### HTML text / attribute values

Use `htmlspecialchars()` with UTF-8.

### URL parameters

Use proper URL/query encoding; do not HTML-escape and assume the URL is safe.

### JavaScript data

Prefer JSON serialization into a safe script block or data endpoint rather than string concatenation.

### Rich HTML

Sanitize against a strict allowlist. Consider storing source separately from sanitized/rendered output if editing matters.

## File uploads

Product/review/admin uploads are a major attack surface.

MUST:

- enforce size limits at PHP/server/application levels;
- inspect file type/content, not only extension;
- generate server-side filenames/storage keys;
- prevent path traversal;
- restrict allowed image/document types;
- strip or re-encode images where appropriate;
- store original uploads outside executable/public web paths when possible;
- serve through controlled URLs/CDN or safe derived images;
- never execute uploaded files;
- apply authorization to private documents;
- handle EXIF/privacy concerns where relevant.

Do not accept `.php` because the UI says "image". Do not trust `Content-Type` supplied by the browser alone.

## Image processing

For product images:

- create bounded derivatives/thumbnails;
- preserve aspect ratio;
- guard against decompression bombs/resource exhaustion;
- remove unnecessary metadata;
- store dimensions and type;
- define fallback/placeholder behavior.

## CSV/imports

Treat imports as untrusted bulk input.

- validate every row;
- stage and show validation errors before destructive apply where practical;
- use transactions/chunking appropriately;
- log import actor/time/source;
- protect against spreadsheet formula injection when exporting data back to CSV.

## Webhooks

Payment/courier webhooks need:

- provider signature verification;
- timestamp/replay policy if provider supports it;
- idempotent processing;
- raw payload logging only after sensitive-data review/redaction;
- correlation to known order/payment records;
- explicit unknown-event handling.

Never mark an order paid because a browser returned to `/success?paid=1`.

## Acceptance checks

- [ ] Every state-changing browser endpoint has CSRF strategy.
- [ ] All user-controlled SQL values are bound.
- [ ] Dynamic sort/filter identifiers are allowlisted.
- [ ] Output is encoded for its context.
- [ ] Rich HTML has an allowlist sanitizer.
- [ ] Uploads cannot become executable scripts.
- [ ] Upload names/paths are server-controlled.
- [ ] Webhooks verify provider authenticity and are idempotent.
- [ ] Imports validate rows and preserve auditability.
