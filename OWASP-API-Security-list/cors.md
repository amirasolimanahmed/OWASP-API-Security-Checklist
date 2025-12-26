
# CORS

## Objective
Configure Cross-Origin Resource Sharing (CORS) safely to prevent unintended cross-domain access and data exposure.

## Summary
CORS is an HTTP-header based mechanism that allows a server to indicate which origins (domain, scheme, port) a browser should permit loading resources from.  
Browsers may send a **preflight** request to check allowed methods/headers before the actual request.

---

## How to test

## 1. Disable CORS headers if cross-domain calls are not supported/expected

### ✅ Expected
- No Access-Control-* headers returned when cross-domain calls are not needed.

### ❌ Fail
- Unnecessary permissive CORS headers enabled

---

## 2. Be as specific as possible and as general as necessary for allowed origins

### ✅ Expected
- Allowed origins are restricted (no wildcard for sensitive endpoints)

### ❌ Fail
- `Access-Control-Allow-Origin: *` used where credentials or sensitive data exist


## CORS Headers

### Request Headers
- Origin
- Access-Control-Request-Method
- Access-Control-Request-Headers

### Response Headers
- Access-Control-Allow-Origin  
  Allows a single origin or wildcard `*` (only safe for non-credential use).
- Access-Control-Allow-Credentials  
  Indicates whether credentialed requests can be exposed.
- Access-Control-Expose-Headers  
  Whitelist headers accessible to JS (e.g., getResponseHeader()).
- Access-Control-Max-Age  
  How long preflight results can be cached.
- Access-Control-Allow-Methods  
  Allowed methods for the resource (preflight response).

---

## Evidence to Collect
- Preflight request/response samples
- Origin allowlist configuration
- Proof of safe behavior for credentialed requests

---

## References
- Wikipedia CORS — https://en.wikipedia.org/w/index.php?title=Cross-origin_resource_sharing&action=edit&section=6
- MDN CORS — https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS
