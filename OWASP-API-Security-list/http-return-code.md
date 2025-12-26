# HTTP Return Code

## Objective
Use semantically correct HTTP status codes for REST APIs, especially for security-related behaviors, to prevent ambiguity and reduce misconfiguration risks.

HTTP defines status codes. When designing REST APIs, do not use only 200 for success or 404 for error. Always use the semantically appropriate status code for the response.

---

## Status Codes Reference (Security-Relevant)

| Code | Message | Description |
|------|---------|-------------|
| 200 | OK | Successful REST action (GET/POST/PUT/PATCH/DELETE). |
| 201 | Created | Resource created; URI returned in Location header. |
| 202 | Accepted | Accepted for processing; not completed yet. |
| 301 | Moved Permanently | Permanent redirection. |
| 304 | Not Modified | Caching-related; client has same copy. |
| 307 | Temporary Redirect | Temporary redirection of resource. |
| 400 | Bad Request | Malformed request (e.g., body format error). |
| 401 | Unauthorized | Wrong or missing authentication. |
| 403 | Forbidden | Authenticated but not permitted. |
| 404 | Not Found | Non-existent resource requested. |
| 405 | Method Not Acceptable | Unexpected HTTP method used (e.g., PUT instead of GET). |
| 406 | Unacceptable | Accept header contains unsupported type. |
| 413 | Payload too large | Request size exceeded limit (e.g., uploads). |
| 415 | Unsupported Media Type | Content type not supported by service. |
| 429 | Too Many Requests | Potential DOS / rate limiting triggered. |
| 500 | Internal Server Error | Unexpected server error; must not reveal internal information. |
| 501 | Not Implemented | Operation not implemented yet. |
| 503 | Service Unavailable | Service temporarily unable to process; retry later. |

---

## Evidence to Collect
- Endpoint-to-status-code mapping (per scenario)
- Examples of error responses showing correct codes without leakage

---

## References
- OWASP API Security Top 10
- OWASP WSTG