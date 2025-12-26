# API Keys

## Objective
Reduce abuse of public/protected REST services by requiring API keys, enforcing rate limits, supporting revocation/rotation, and ensuring API keys are never exposed in URLs or logs.

Public REST services without access control risk being farmed, causing excessive bandwidth/compute bills. API keys can mitigate this risk and help monetize APIs via access plans.  
**Note:** API keys are relatively easy to compromise when issued to third parties, so they must not be the only protection for sensitive resources.

---

## 1. Require API keys for every request to protected endpoints

### How to test
- Call protected endpoints:
  - Without API key
  - With invalid API key
  - With valid API key

### ✅ Expected
- Requests without/invalid key are rejected (401/403 depending on design)
- Requests with valid key are accepted

### ❌ Fail
- Protected endpoint works without a key
- Invalid key is accepted

---

## 2. Return 429 Too Many Requests when requests are too frequent

### How to test
- Send high-rate requests using the same API key.
- Increase the request rate until throttling applies.

### ✅ Expected
- 429 Too Many Requests returned when limits are exceeded

### ❌ Fail
- No throttling / unlimited requests allowed

---

## 3. Revoke API key if client violates usage agreement

### How to test
- Trigger usage policy violation scenarios (rate abuse, prohibited behavior).
- Attempt requests after revocation.

### ✅ Expected
- Revoked key stops working immediately (401/403)

### ❌ Fail
- Revoked key continues to work

---

## 4. Do not rely exclusively on API keys for sensitive/high-value resources

### How to test
- Identify sensitive endpoints and verify they also require:
  - Authentication (JWT/OAuth/session/etc.)
  - Authorization (RBAC/ABAC/scopes)

### ✅ Expected
- Sensitive endpoints require more than an API key (defense-in-depth)

### ❌ Fail
- API key alone grants access to sensitive resources

---

## Other Useful Checks

## 5. Test for missing or invalid API key

### How to test
- Send requests:
  - Without API key
  - With a fake key

### ✅ Expected
- 401 Unauthorized

### ❌ Fail
- Request succeeds without a valid key

---

## 6. Check key exposure in URL

### How to test
- Verify the API key is not visible in:
  - Query parameters
  - Logs
  - Browser history

### ✅ Expected
- Key is sent in headers (not URL)

### ❌ Fail
- Key appears in URL or logs

---

## 7. Check IP restrictions

### How to test
- Test whether the API key works from:
  - Allowed IP (should succeed)
  - Blocked IP (should fail)

### ✅ Expected
- IP restrictions enforced as configured

### ❌ Fail
- Key works from blocked IPs

---

## 8. Test key rotation

### How to test
- Request two different API keys and test:
  - Old key stops working
  - New key works

### ✅ Expected
- API supports safe rotation without downtime

### ❌ Fail
- Old key still works after rotation
- Rotation breaks legitimate traffic unexpectedly

---

## 9. Check log activity

### How to test
- Make multiple API calls and verify logs show:
  - Timestamp
  - User/client
  - Endpoint
  - Count

### ✅ Expected
- Traceability exists for usage monitoring

### ❌ Fail
- No usage traces, or logs contain sensitive secrets

---

## 10. Test expiration

### How to test
- Send a request using an expired key.

### ✅ Expected
- 401 Unauthorized or 403 Forbidden

### ❌ Fail
- Expired key still works

---

## 11. Test encryption security

### How to test
- Verify key is transmitted only via:
  - HTTPS + TLS 1.2 or higher
- Attempt HTTP calls if accessible.

### ✅ Expected
- No API key accepted over HTTP
- TLS enforced

### ❌ Fail
- Key is accepted over HTTP

---

## 12. Test privilege scope

### How to test
- Create multiple keys with different scopes:
  - Read-only
  - Write-only
  - Admin
- Verify they cannot cross-access.

### ✅ Expected
- Scope boundaries enforced

### ❌ Fail
- Read-only key can write/delete
- Non-admin key can perform admin operations

---

## 13. Brute-force protection

### How to test
- Try random API keys rapidly.

### ✅ Expected
- 429 Too Many Requests
- IP block / throttling

### ❌ Fail
- Unlimited attempts allowed

---

## Evidence to Collect
- Requests/responses for missing/invalid keys
- Rate-limit proof (429)
- Proof of rotation/revocation behavior
- Logs showing traceability without secrets

---

## References
- OWASP API Security Top 10
- OWASP WSTG
