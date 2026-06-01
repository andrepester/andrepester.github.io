# Security Header Strategy

This site is hosted on GitHub Pages for `andre-pester.de`. The chosen hosting
variant remains GitHub Pages without a CDN, reverse proxy, or hosting migration.

GitHub Pages does not provide a repository-level way to configure custom HTTP
response headers for this static site. As a result, the following headers cannot
be set directly under the chosen hosting constraints:

- `Strict-Transport-Security`
- `Content-Security-Policy` as an HTTP response header
- `X-Content-Type-Options`
- `Permissions-Policy`
- `X-Frame-Options`
- CSP `frame-ancestors`

GitHub Pages still provides HTTPS delivery and redirects HTTP requests to HTTPS
when "Enforce HTTPS" is enabled. A custom HSTS policy, including `max-age`,
`includeSubDomains`, or `preload`, is not controlled by this repository.

The repository applies the browser hardening that can be expressed safely in
static HTML:

- `index.html` sets a CSP via `meta http-equiv="Content-Security-Policy"`.
- The policy allows only local images and local CSS.
- The inline JSON-LD person metadata is allowed with a SHA-256 hash in
  `script-src`.
- `index.html` sets `Referrer-Policy` via
  `meta name="referrer" content="strict-origin-when-cross-origin"`.

If the JSON-LD block changes, recalculate the CSP hash from the exact text
between `<script type="application/ld+json">` and `</script>` before deploying.

This meta-based CSP is intentionally treated as partial hardening, not as a full
replacement for response headers. In particular, frame protection via
`frame-ancestors` cannot be delivered by a CSP meta tag.
