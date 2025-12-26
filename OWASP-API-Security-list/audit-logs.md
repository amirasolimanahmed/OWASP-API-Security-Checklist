# Audit Logs

## Objective
Ensure security-relevant events are logged consistently for incident investigation, monitoring, and compliance, while preventing sensitive data exposure and log injection.

---

## 1. Write audit logs before and after security-related events

### What to verify
- Security-relevant actions are consistently logged to support incident investigation and compliance.

### How to test
- Perform security-sensitive actions:
  - Login (success & failure)
  - Token issuance / refresh / revocation
  - Password reset / change
  - Role or permission changes
  - Create / update / delete critical resources
- Trigger both successful and failed scenarios.

### ✅ Expected
- Audit log entry exists for each action.
- Log includes:
  - Timestamp
  - Event name/type
  - Result (success / failure)
  - Actor (user ID / service account)
  - Target resource or user
  - Correlation / request ID
- ❌ No passwords, secrets, or full tokens logged.

### ❌ Fail
- Missing audit logs for security events
- Secrets/tokens/passwords appear in logs
- No correlation ID or missing actor identity

---

## 2. Consider logging token validation errors to detect attacks

### What to verify
- Authentication and authorization failures are logged for attack detection without exposing sensitive data.

### How to test
- Call protected endpoints using:
  - Expired token
  - Invalid or tampered token
  - Token with wrong scope / role
  - Missing token
- Repeat requests to simulate brute-force or token abuse.

### ✅ Expected
- Audit/security logs contain:
  - Token validation failure event
  - Error category (expired, invalid, insufficient scope)
  - Endpoint and HTTP method
  - Source IP and client/application ID
- Client response remains generic (401 / 403).
- ❌ Tokens are never written to logs.

### ❌ Fail
- No logging of repeated auth failures
- Tokens or sensitive auth material logged

---

## 3. Prevent log injection attacks by sanitizing log data

### What to verify
- User-controlled input cannot manipulate or corrupt log entries.

### How to test
- Send malicious input containing:
  - Newlines (`\n`, `\r`)
  - Fake log entries
  - JSON-breaking characters
  - Extremely long strings
- Inject these values via:
  - Headers (User-Agent, X-Forwarded-For)
  - Query parameters
  - Request body fields

### ✅ Expected
- Logs remain well-formed (one entry per event).
- Special characters are escaped or sanitized.
- Long inputs are truncated safely.
- ❌ No forged or injected log entries appear.

### ❌ Fail
- Log entries can be split/forged via newline injection
- Logs become corrupted/unparseable

---

## Evidence to Collect
- Audit log samples (sanitized)
- Proof of correlation/request IDs
- Examples showing sensitive values are not logged
- Examples showing sanitization/truncation

---

## References
- OWASP Logging Cheat Sheet
- OWASP API Security Top 10
- ISO/IEC 27001:2022 – Logging and monitoring controls
