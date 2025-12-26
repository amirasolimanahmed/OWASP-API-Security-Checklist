# Input Validation

## Objective
Ensure all inputs are validated and sanitized to prevent injection, parsing, and abuse scenarios, and apply limits to reduce denial-of-service risks.

---

## Checklist

## 1. Do not trust input parameters/objects

### ✅ Expected
- All inputs are treated as untrusted and validated server-side.

### ❌ Fail
- Client-side validation only / blind trust in inputs

---

## 2. Validate input: length / range / format and type

### ✅ Expected
- Validation enforced for:
  - length, range, format, type

### ❌ Fail
- Missing validation or weak validation

---

## 3. Use strong types (numbers, booleans, dates, fixed ranges)

### ✅ Expected
- Implicit validation via strong typing and strict schemas

### ❌ Fail
- Loose/unsafe parsing (e.g., strings everywhere)

---

## 4. Constrain string inputs with regexps

### ✅ Expected
- Regex used where appropriate to restrict values

### ❌ Fail
- Free-form strings accepted when not required

---

## 5. Reject unexpected/illegal content

### ✅ Expected
- Unexpected fields and illegal content rejected

### ❌ Fail
- Extra fields silently accepted when unsafe

---

## 6. Use validation/sanitation libraries or frameworks

### ✅ Expected
- Standard validation libraries used consistently

### ❌ Fail
- Custom ad-hoc validation everywhere

---

## 7. Enforce request size limit; reject with 413 Payload Too Large

### ✅ Expected
- Oversized requests rejected with **413**

### ❌ Fail
- No request size limits; server performance degraded

---

## 8. Consider logging input validation failures

### ✅ Expected
- Validation failures logged for detection (without sensitive data)
- High-rate failures treated as suspicious

### ❌ Fail
- No monitoring of repeated invalid requests

---

## 9. Refer to input validation cheat sheet

### ✅ Expected
- Validation approach aligned to established guidance

---

## 10. Use a secure parser (protect against XXE if XML)

### ✅ Expected
- Secure parsers used
- If XML: XXE protections enabled

### ❌ Fail
- XXE-vulnerable parsers or unsafe parsing configurations

---

## Best Practices (Robot Framework)
Call your API by sending invalid data (data type, length, format, etc.) and assert:
- returned error code **400**
- error message **Bad Request**

---

## Evidence to Collect
- Negative test cases and responses (400)
- Size limit tests (413)
- Logs showing validation failures without leakage

---

## References
- OWASP Input Validation Cheat Sheet
- OWASP API Security Top 10
