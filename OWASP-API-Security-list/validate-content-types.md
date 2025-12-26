# Validate Content Types

## Objective
Ensure request and response bodies match intended content types, accept only documented types, and prevent content-type confusion or header injection.

A REST request/response body should match the intended content type header. Otherwise this could cause misinterpretation and lead to code injection/execution.

**N.B:** Document all supported content types in your API.

---

## Validate Request Content Types

## 1. Reject missing or unexpected Content-Type headers (406 / 415)

### What to verify
Requests must include a valid Content-Type header for payloads (POST/PUT/PATCH), and only supported types are accepted.

### How to test
- Send POST/PUT/PATCH with a body but:
  - No Content-Type header
  - Content-Type: text/plain
  - Content-Type: application/xml (if XML not supported)
  - Content-Type: application/javascript
  - Content-Type: multipart/form-data (if not supported)
- Send malformed content-type values:
  - Content-Type: application/json; charset=bad
  - Content-Type: application/json, text/html (invalid)
- Send JSON body with wrong header:
  - Body: `{"a":1}` + Content-Type: text/plain

### ✅ Expected
- API rejects with 415 Unsupported Media Type (preferred) or 406 Not Acceptable
- Response message is generic (no stack traces)
- Server does not attempt to parse/execute unexpected formats

### ❌ Fail
- Server accepts unsupported types
- Parsing/execution attempted for unexpected formats

---

## 2. Accept only documented/allowed request content types

### What to verify
Only content types listed in API documentation are accepted.

### How to test
- Collect allowed types (example):
  - application/json (and optionally application/problem+json)
- For each endpoint, send the same request with:
  - allowed type → should succeed
  - not allowed type → should fail

### ✅ Expected
- Success for allowed content types
- Reject for others with 415/406

### ❌ Fail
- Undocumented types accepted

---

## Send Safe Response Content Types

## 3. Response Content-Type must match the real response body

### What to verify
Response headers accurately represent the returned payload type.

### How to test
- Call endpoints and check:
  - Content-Type header matches actual body format
    - JSON body ⇒ application/json
    - error JSON ⇒ application/problem+json (if used)
- Try to force mismatches by using Accept values like:
  - Accept: text/html
  - Accept: application/javascript
  - Accept: */*

### ✅ Expected
- Response Content-Type is always correct (JSON -> application/json)
- ❌ Never respond with text/html or application/javascript unless explicitly supported & safe
- ❌ No “content-type confusion” (e.g., JSON sent as HTML/JS)

### ❌ Fail
- Response header/body mismatch
- Unsafe content types returned unexpectedly

---

## 4. Do NOT copy Accept directly into response Content-Type

### What to verify
Server chooses from a whitelist of safe types; it doesn’t reflect arbitrary Accept input.

### How to test
- Send requests with:
  - Accept: application/javascript
  - Accept: text/html
  - Accept: image/png
- Observe the response Content-Type.

### ✅ Expected
- If unsupported accept type:
  - reject with 406 Not Acceptable, OR
  - return default safe type (commonly application/json)
- ❌ Response must not become application/javascript just because Accept asked for it

### ❌ Fail
- Content-Type is reflected from attacker-controlled Accept header

---

## 5. Reject unsupported Accept (if enforcing negotiation)

### What to verify
If the endpoint supports negotiation, unsupported Accept is rejected.

### How to test
- For an endpoint that returns JSON only:
  - Accept: application/xml
  - Accept: application/javascript

### ✅ Expected
- 406 Not Acceptable (recommended if enforcing Accept)
- OR safely return application/json (if API ignores Accept)
- In both cases: no unsafe content type returned

### ❌ Fail
- Unsafe response type returned due to Accept manipulation

---

## 6. Prevent header injection via Accept / Content-Type

### What to verify
Headers are validated; attackers can’t inject extra headers/values.

### How to test
- Send (via tools allowing raw headers):
  - `Accept: application/json\r\nX-Evil: 1`
  - `Content-Type: application/json\r\nX-Evil: 1`
- Also test very long Accept headers.

### ✅ Expected
- Request rejected (400/406/415)
- No injected headers appear in the response

### ❌ Fail
- Response contains injected headers/values

---

## Evidence to Collect
- Requests with valid/invalid headers
- Response codes (415/406) and content-type correctness
- Proof of header-injection prevention

---

## References
- OWASP API Security Top 10
- OWASP WSTG
