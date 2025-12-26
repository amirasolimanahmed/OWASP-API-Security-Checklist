# Security Headers

## Objective
Ensure required security headers are included in API responses to reduce risks such as caching of sensitive data, MIME sniffing, and clickjacking.

Client-side testing concerns execution of code in the client (browser). APIs should include security headers to reduce exposure if responses are rendered or processed by browsers/tools.

---

## Required Headers (All API Responses)

| Header | Rationale |
|--------|-----------|
| Cache-Control: no-store | Prevent sensitive information from being cached. |
| Content-Security-Policy: frame-ancestors 'none' | Protect against clickjacking. |
| Content-Type | Specify response content type (JSON responses should be application/json). |
| Strict-Transport-Security | Require HTTPS and protect against spoofed certificates. |
| X-Content-Type-Options: nosniff | Prevent MIME sniffing and HTML interpretation. |
| X-Frame-Options: DENY | Protect against clickjacking. |

---

## Additional Headers (If Responses May Be Rendered as HTML)
If the API will never return HTML, these may not be necessary. If uncertain, include them as defense-in-depth.

| Header | Rationale |
|--------|-----------|
| Content-Security-Policy: default-src 'none' | CSP mainly affects HTML-rendered pages. |
| Feature-Policy: 'none' | Feature policies mainly affect HTML. |
| Referrer-Policy: no-referrer | Avoid extra requests from non-HTML responses. |

---

## Best Practices (Robot Framework)
In automation scripts, call an API and assert that all required headers are returned with valid values.

---

## Evidence to Collect
- Header assertions from automated tests
- Response samples showing headers across endpoints

---

## References
- OWASP Secure Headers Project — https://owasp.org/www-project-secure-headers/
