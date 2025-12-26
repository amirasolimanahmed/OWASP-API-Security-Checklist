# HTTPS

## Objective
Ensure REST services provide HTTPS endpoints only, protecting authentication credentials in transit and ensuring confidentiality and integrity.

Secure REST services must only provide HTTPS endpoints. This protects authentication credentials in transit (passwords, API keys, JWTs), allows clients to authenticate the service, and guarantees integrity of transmitted data.

Consider mutually authenticated client-side certificates for highly privileged services.

See: Transport Layer Protection Cheat Sheet  
https://cheatsheetseries.owasp.org/cheatsheets/Transport_Layer_Protection_Cheat_Sheet.html

---

## SSL vs TLS
SSL was the original protocol used to encrypt HTTP traffic (HTTPS). SSL versions 2 and 3 have serious cryptographic weaknesses and should no longer be used.

TLS (effectively SSL 3.1) replaced SSL:
- TLS 1.0
- TLS 1.1
- TLS 1.2
- TLS 1.3

The terms "SSL", "SSL/TLS" and "TLS" are often used interchangeably, but TLS is the modern standard.

---

## Tools

### Online Tools
- SSL Labs Server Test — https://www.ssllabs.com/ssltest
- CryptCheck — https://cryptcheck.fr/
- CypherCraft — https://www.cyphercraft.io/
- Hardenize — https://www.hardenize.com/
- ImmuniWeb — https://www.immuniweb.com/ssl/
- Observatory by Mozilla — https://observatory.mozilla.org/
- SSL Configuration Generator — https://ssl-config.mozilla.org/

### Offline Tools
- O-Saft (OWASP) — https://wiki.owasp.org/index.php/O-Saft
- CipherScan — https://github.com/mozilla/cipherscan
- CryptoLyzer — https://gitlab.com/coroner/cryptolyzer
- SSLScan — https://github.com/rbsec/sslscan
- SSLyze — https://github.com/nabla-c0d3/sslyze
- testssl.sh — https://testssl.sh/
- tls-scan — https://github.com/prbinu/tls-scan

---

## Evidence to Collect
- Proof that HTTP is disabled or redirected safely
- TLS version/cipher scan results
- HSTS configuration evidence (if applicable)

---

## References
- OWASP Transport Layer Protection Cheat Sheet
- OWASP API Security Top 10
