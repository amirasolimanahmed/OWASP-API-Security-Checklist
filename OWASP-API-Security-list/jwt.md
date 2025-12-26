# JWT

## Objective
Ensure JSON Web Tokens (JWTs) are securely issued and validated, protected against algorithm confusion, and verified using trusted server-side configuration.

## What is JSON Web Token (JWT)?
JWT (RFC 7519) is a compact and self-contained way to securely transmit information between parties as a JSON object. JWTs can be digitally signed using:
- HMAC (secret key)
- RSA/ECDSA (public/private key)

More details: https://jwt.io/introduction

---

## JWT Structure
A JWT has three parts separated by dots:
- Header
- Payload
- Signature

Example:
`xxxxx.yyyyy.zzzzz`

### Header
Example:
```json
{
  "alg": "HS256",
  "typ": "JWT"
}
Payload
Claims about the entity and additional data.
Example:
{
  "sub": "1234567890",
  "name": "John Doe",
  "admin": true
}
Signature
Created using the encoded header + payload and a secret/key using the algorithm.
________________________________________
How to test
1. Reject unsecured JWTs with {"alg":"none"}
Ensure JWTs are integrity protected by signature or MAC. Do not allow unsecured JWTs.
•	https://tools.ietf.org/html/rfc7519#section-6.1
________________________________________
2. Prefer signatures over MACs
Signatures should be preferred over MACs for integrity protection.
________________________________________
3. If MACs are used, all validators can also mint tokens
If MACs are used, any service that can validate can create new JWTs using the same key.
A compromise of any service compromises all services sharing the key.
•	https://tools.ietf.org/html/rfc7515#section-10.5
________________________________________
4. Validate JWT integrity and claims server-side (do not trust header to pick algorithm)
A relying party must verify integrity based on its own configuration/hard-coded logic and must not rely on the JWT header to select the verification algorithm.
•	https://www.chosenplaintext.ca/2015/03/31/jwt-algorithm-confusion.html
•	https://www.youtube.com/watch?v=bW5pS4e_MX8
Verify at least these standard claims:
1.	iss (issuer) — trusted issuer and expected signing key owner
2.	aud (audience) — token intended for this relying party
3.	exp (expiration time) — current time before expiration
4.	nbf (not before) — current time after validity starts
When a session is terminated early (logout/idle timeout), invalidate associated JWTs until expiry using a blacklist (digest/hash submitted to API).
•	https://cheatsheetseries.owasp.org/cheatsheets/JSON_Web_Token_for_Java_Cheat_Sheet.html#token-explicit-revocation-by-the-user
________________________________________
Tools
•	OpenTelemetry — https://opentelemetry.io/
•	JWT intro — https://jwt.io/introduction
•	ZAP JWT Scanner — https://www.zaproxy.org/blog/2020-09-03-zap-jwt-scanner/
•	myjwt — https://myjwt.readthedocs.io/en/latest/
•	MyJWT — https://github.com/mBouamama/MyJWT
________________________________________
Evidence to Collect
•	Proof that alg=none is rejected
•	Claim validation tests (iss/aud/exp/nbf)
•	Algorithm confusion tests
•	Token revocation/blacklist behavior evidence
________________________________________
References
•	RFC 7519 (JWT)
•	OWASP JWT Cheat Sheet