# Sensitive Information in HTTP Requests

## Objective
Prevent leaking credentials such as passwords, tokens, and API keys via URLs, logs, caching, or browser history.

RESTful web services should prevent leaking credentials. Passwords, security tokens, and API keys should not appear in the URL, as this can be captured in web server logs and becomes intrinsically valuable.

- In POST/PUT requests, sensitive data should be transferred in the request body or request headers.
- In GET requests, sensitive data should be transferred in an HTTP header.

✅ OK:
- https://example.com/resourceCollection/[ID]/action
- https://twitter.com/vanderaj/lists

❌ NOT OK:
- https://example.com/controller/123/action?apiKey=a53f435643de32  
(because the API key is in the URL)

---

## 1. Ensure sensitive data is never sent in URLs

### What to verify
- Credentials and secrets are not exposed via URL paths or query parameters.

### How to test
- Inspect all API calls using:
  - Browser DevTools
  - Postman / Curl
  - Proxy tools (Burp, OWASP ZAP)
- Look for sensitive data in:
  - URL path
  - Query parameters

Sensitive data includes:
- Passwords
- API keys
- Access / refresh tokens
- Session IDs
- Private identifiers

### ✅ Expected
- URLs contain only:
  - Resource identifiers
  - Non-sensitive parameters
- ❌ No credentials or secrets appear in the URL.
- URLs are safe to log and cache.

### ❌ Fail
- Any credential/secret appears in path or query parameters

---

## 2. Verify POST/PUT requests send sensitive data in body or headers

### What to verify
- Sensitive data is transferred securely using the request body or HTTP headers.

### How to test
- Send POST / PUT / PATCH requests containing:
  - Login credentials
  - Tokens
  - API keys
- Inspect request payload and headers.

### ✅ Expected
- Sensitive data appears only in:
  - Request body (JSON/form)
  - Authorization header (e.g. `Authorization: Bearer <token>`)
- ❌ Sensitive data is not duplicated in the URL.

### ❌ Fail
- Secrets are present in URL or duplicated in URL

---

## 3. Verify GET requests do not expose sensitive data

### What to verify
- GET requests never carry sensitive information in URLs.

### How to test
- Execute GET requests and inspect:
  - Query parameters
  - Headers

### ✅ Expected
- GET requests use:
  - Headers for authentication (Authorization)
  - Query params only for filtering/sorting (non-sensitive)
- ❌ No credentials or secrets in query parameters.

### ❌ Fail
- Sensitive data passed in query parameters

---

## 4. Check server logs for accidental exposure

### What to verify
- Web server and application logs do not capture sensitive data.

### How to test
- Execute authenticated requests.
- Review:
  - Access logs
  - Reverse proxy logs
  - Application logs

### ✅ Expected
- Logged URLs do not contain secrets.
- Headers and bodies containing sensitive data are masked or excluded.
- ❌ No tokens, passwords, or API keys in logs

### ❌ Fail
- Secrets/tokens appear in logs

---

## Evidence to Collect
- Sample requests showing headers/body usage
- Logs showing safe URLs and masked secrets

---

## References
- OWASP API Security Top 10
- OWASP Logging Cheat Sheet
