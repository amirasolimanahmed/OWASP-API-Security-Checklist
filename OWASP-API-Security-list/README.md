# API Security Checklist

This repository contains security checklists and “how to test” guidance for REST APIs.  
Each topic is documented in its own folder with a dedicated `README.md`.

---

## ✅ Index

### Core API Security Topics
- [Management Endpoints](./management-endpoints.md)
- [Error Handling](./error-handling.md)
- [Audit Logs](./audit-logs.md)
- [Restrict HTTP Methods](./restrict-http-methods.md)
- [Sensitive Information in HTTP Requests](./sensitive-information-in-http-requests.md)
- [Validate Content Types](./validate-content-types.md)
- [API Keys](./api-keys.md)
- [HTTP Return Code](./http-return-code.md)

### Security Foundations
- [Access Control](./access-control.md)
- [Input Validation](./input-validation.md)
- [HTTPS](./https/README.md)
- [Security Headers](./security-headers.md)

### Authentication & Web Security
- [JWT](./jwt.md)
- [CORS](./cors.md)

---

## 📌 How to Use
1. Pick a topic from the index above.
2. Follow the **How to Test** section.
3. Mark findings as ✅ Expected or ❌ Fail.
4. Collect evidence (logs, requests/responses, screenshots).
5. Add automation where possible (Robot Framework / CI).

---

## ✍️ Contributing
- Keep the same structure across all topics:
  - Objective
  - Checklist items (numbered)
  - What to verify
  - How to test
  - ✅ Expected / ❌ Fail
  - Evidence to collect
  - References

---

## References
- OWASP API Security Top 10
- OWASP Web Security Testing Guide
- ISO/IEC 27001:2022
