# Access Control

## Objective
Ensure every API endpoint enforces authorization (access control) correctly, locally, and consistently, with centralized authentication and least-privilege authorization rules.

## What is Access Control / Authorization
Authorization determines whether a request to access a resource is granted or denied.  
Authorization is not equivalent to authentication:
- **Authentication** = validating identity
- **Authorization** = validating permissions

Web applications require access controls for users with varying privileges, and admins to manage permissions. The correct model depends on risk assessment and threat modeling.

---

## Key Principles
- Non-public REST services must perform access control at each endpoint.
- In modern architectures (microservices):
  1. Access control decisions should be taken locally by endpoints to minimize latency and coupling.
  2. Authentication should be centralized in an Identity Provider (IdP) that issues access tokens.

---

## Best Practices (Robot Framework)
Use Robot Framework to call an API with a user who is not authorized to do the action and assert:
- response error code **403**
- error message **Forbidden**

---

## Evidence to Collect
- Tests proving 401 vs 403 behavior
- Role/scope matrix for endpoints
- Logs/trace IDs for denied access

---

## References
- OWASP Authorization Cheat Sheet Series
- OWASP API Security Top 10
