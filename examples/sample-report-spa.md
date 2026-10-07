# Example preflight report — simulated data

> This is a fictional example for illustrating report format. No live site was tested.

## Verdicts

- `shop.example.test` — PASS WITH FIXES
- Overall — PASS WITH FIXES

## Scope and request count

- Authorized HTTPS release preflight for `https://shop.example.test`
- 14 HTTP requests, including redirects
- No authentication, form submission, uploads, exploitation, or port scanning
- robots.txt was readable; none of the requested paths below were disallowed

## What was checked

- HTTPS homepage, HTTP-to-HTTPS behavior, robots.txt, and favicon
- Fixed sensitive-path checks permitted by robots.txt
- Homepage-referenced JavaScript asset and its corresponding source map
- Relevant response headers
- One benign nonexistent path as a catch-all comparison

## Confirmed findings

- **MEDIUM — Source map accessible.** The homepage-referenced production JavaScript asset had a corresponding accessible source map. The inspected bounded response contained source structure and comments; no credentials or other sensitive material were confirmed.
- **LOW — HSTS missing.** The HTTPS homepage response did not include Strict-Transport-Security.

## Expected scanner traffic / false positives

- `/.env` returned 200, but its final URL, content type, body fingerprint, and response structure matched the homepage shell.
- .env-style probes return the SPA shell rather than the requested file; no .env exposure was established.
- No webshell-name probes were sent.

## WAF/CDN limitations

- No blanket challenge response was observed.
- The origin was assessable for the checks listed above.

## Evidence

| Path | Status | Content type | Size/fingerprint | Assessment |
|---|---:|---|---|---|
| `/` | 200 | `text/html` | 4,812 bytes; fingerprint `fp-home-01` | Baseline |
| `/.env` | 200 | `text/html` | 4,812 bytes; fingerprint `fp-home-01` | Same SPA shell as baseline |
| `/assets/app.example.js.map` | 200 | `application/json` | 18,340 bytes; fingerprint `fp-map-01` | Accessible source map; no secret confirmed |

Fingerprints above are fictional placeholders. No raw sensitive response bodies or secret values are included.

## Fixes ordered by priority

1. Review whether publishing the production source map is intentional; disable it or restrict access if source disclosure is not desired.
2. Add HSTS after confirming HTTPS is consistently available for the relevant host and subdomains.

## Exact re-checks after fixes

1. Request the corresponding source-map URL and verify it is no longer publicly accessible if access is meant to be restricted.
2. Request `https://shop.example.test/` and verify the HSTS header is present with the intended policy.
3. Repeat the permitted `/.env` comparison and confirm it still serves the SPA shell or an explicit 404/403 response, not a configuration file.
