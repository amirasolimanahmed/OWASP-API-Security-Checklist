# Management Endpoints

## Objective
Ensure that management and administrative endpoints are properly secured, not exposed publicly, and accessible only to authorized users through controlled and monitored access mechanisms.

---

## 1. Avoid exposing management endpoints via Internet

### How to Test
- Identify all management endpoints from:
  - API documentation
  - OpenAPI / Swagger
  - Code / configuration (e.g. `management.endpoints.*`, `actuator`)
  - Framework default endpoints
- Try accessing endpoints using browser, curl, or Postman  
  Example:
  ```bash
  curl https://example.com/actuator/health

### ✅ Expected
•	Endpoints are not reachable
•	HTTP 403 / 404 or connection refused

### ❌ Fail
•	Management endpoint returns data publicly
•	Endpoint accessible without restriction
________________________________________
## 2. Restrict management endpoints with strong authentication (e.g. MFA)
How to Test
•	Attempt access:
o	Without authentication
o	With invalid credentials
o	With valid credentials but without MFA
•	Verify:
o	MFA is enforced (OTP, hardware token, SSO with MFA)
o	Session timeout is configured
o	Multiple failed login attempts are handled

### ✅ Expected
•	401 Unauthorized without authentication
•	403 Forbidden with insufficient role
•	MFA challenge required

### ❌ Fail
•	Single-factor authentication only
•	Reusable tokens without expiration
•	Access granted without MFA
________________________________________
## 3. Expose management endpoints on a different port or host
How to Test
•	Check listening ports using:
o	netstat -tulnp
o	ss -lntup
•	Verify:
o	Management endpoints are not running on the same port as public APIs
o	Bound to internal or private network
•	Attempt external access

### ✅ Expected
•	Only accessible from internal network or VPN
•	Separate port or network interface used

### ❌ Fail
•	Same host and port exposed publicly
•	Management endpoints accessible from the Internet
________________________________________
## 4. Restrict access using firewall rules or ACLs
How to Test
•	Review firewall or security group rules:
o	IP allowlists
o	VPN-only access
•	Test access from:
o	Allowed IP range
o	Non-allowed IP range
•	Validate rules in:
o	AWS Security Groups
o	Azure NSG
o	GCP Firewall rules

### ✅ Expected
•	Only trusted IP ranges allowed
•	VPN-only access enforced

### ❌ Fail
•	0.0.0.0/0 allowed for management ports
•	No firewall or ACL restrictions
________________________________________
## 5. Role-Based Access Control (RBAC)
How to Test
•	Login using different roles:
o	Admin
o	Operator
o	Read-only
•	Verify permissions per role
•	Attempt administrative actions with non-admin roles
### ✅ Expected
•	Least-privilege access enforced
•	Only authorized roles can perform admin actions

### ❌ Fail
•	Any authenticated user can perform admin actions
•	No role separation on management endpoints
________________________________________
Evidence to Collect
•	Sanitized request and response samples
•	Firewall / security group configuration screenshots
•	Authentication and MFA configuration
•	Logs showing blocked access attempts
________________________________________
References
•	OWASP API Security Top 10
•	OWASP Web Security Testing Guide
•	ISO/IEC 27001:2022 – Access Control
