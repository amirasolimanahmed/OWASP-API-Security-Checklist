# Restrict HTTP Methods

## Objective
Ensure each endpoint only allows required HTTP methods, rejects others safely, and enforces authorization per method to reduce attack surface.

---

## 1. Apply a whitelist of permitted HTTP methods (e.g. GET, POST, PUT)

### ✅ What this means
Each API endpoint must explicitly allow only the HTTP methods it needs.  
All other methods must be disabled by design, not just ignored.

Example:
- GET `/users/{id}` → allowed  
- POST `/users` → allowed  
- DELETE `/users/{id}` → allowed  
- PATCH `/users/{id}` → NOT allowed  
- OPTIONS `/users/{id}` → restricted or controlled  
- TRACE `/users/{id}` → NEVER allowed  

### 🚨 Why this matters (Security Risk)
Attackers abuse unused methods like:
- PUT, PATCH → unauthorized updates
- DELETE → data loss
- TRACE → Cross-Site Tracing (XST)
Some frameworks enable extra methods by default.

### How to test
- Send requests using all HTTP methods:
  - GET, POST, PUT, DELETE, PATCH, OPTIONS, TRACE

### ✅ Expected
- Only explicitly allowed methods are accepted.
- Dangerous methods (TRACE, CONNECT) are disabled.

### ❌ Fail
- Unsupported methods succeed
- TRACE/CONNECT enabled

---

## 2. Reject non-whitelisted methods with 405 Method Not Allowed

### ✅ What this means
If a client sends a request using a method that is not allowed, the API must reject it and return:
- HTTP/1.1 405 Method Not Allowed
- Allow: GET, POST

❌ NOT acceptable:
- 200 OK
- 404 Not Found (hides misconfiguration)
- 500 Internal Server Error

### How to test
- Call an endpoint with non-allowed methods (PATCH/TRACE/etc.)
- Check status code and Allow header.

### ✅ Expected
- 405 Method Not Allowed
- Allow header contains permitted methods

### ❌ Fail
- 200/404/500 returned for disallowed method

---

## 3. Ensure caller is authorized per method (collection/action/record)

### ✅ What this means
Authorization must be checked per method, not just per endpoint.

Example:
- GET `/users/1` → allowed for USER role
- DELETE `/users/1` → allowed only for ADMIN
- POST `/users` → allowed only for SERVICE accounts

❌ Common mistake:
“User is authenticated, so all methods are allowed”

### How to test
- Use different roles to call the same resource using different methods.
- Verify permission boundaries per method.

### ✅ Expected
- Authorization enforced per HTTP method and role.

### ❌ Fail
- Authenticated users can perform methods they should not

---

## By Using Robot Framework
- Send requests using all HTTP methods (GET, POST, PUT, DELETE, PATCH, OPTIONS, TRACE).
- Verify only explicitly allowed methods are accepted.
- Ensure non-whitelisted methods return 405 (Method Not Allowed).
- Validate authorization is enforced per HTTP method and role.
- Verify dangerous methods (TRACE, CONNECT) are disabled.

---

## Evidence to Collect
- Endpoint/method matrix results
- Response codes and Allow headers
- Role-based method authorization results

---

## References
- OWASP API Security Top 10
- OWASP WSTG