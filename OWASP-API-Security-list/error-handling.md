# Error Handling

## Objective
Ensure the API handles errors securely by returning generic messages, preventing information leakage, and avoiding exposure of internal implementation details.

---

## 1. Respond with generic error messages (avoid revealing failure details)

### What to verify
- API responses must not reveal internal implementation details.

### How to test
- Send malformed requests:
  - Invalid JSON (missing braces, wrong data types)
  - Missing required fields
  - Extra unexpected fields
- Trigger server-side errors intentionally:
  - Divide-by-zero–like inputs
  - Invalid object references (e.g. non-existing IDs)

### ✅ Expected
- Response contains generic message like:
  - “An error occurred”
  - “Invalid request”
- Only HTTP status codes returned (400, 401, 403, 500)
- ❌ No stack traces
- ❌ No framework/library names (Spring, Hibernate, Express, etc.)
- ❌ No database errors (SQL syntax, table names)

### ❌ Fail
- Detailed technical error messages returned to client
- Stack traces or internal debug information exposed
- Database/framework details revealed

---

## 2. Do not pass technical details (call stacks or internal hints) to the client

### What to verify
- Internal debugging information is never exposed to clients.

### How to test
- Cause runtime exceptions:
  - Null values
  - Out-of-range numbers
  - Unsupported enum values
- Inspect response body and headers.

### ✅ Expected
- ❌ No stack traces
- ❌ No file paths (`/usr/src/app/...`)
- ❌ No class/method names
- ❌ No environment details (OS, JVM, Node version)

### ❌ Fail
- Runtime exception details exposed to the client
- Internal paths/class names leaked

---

## Evidence to Collect
- Sample error responses (sanitized)
- HTTP status codes observed per test case
- CI logs / test logs proving no internal info leaked

---

## References
- OWASP API Security Top 10
- OWASP Web Security Testing Guide
- ISO/IEC 27001:2022 – Secure development / logging considerations
