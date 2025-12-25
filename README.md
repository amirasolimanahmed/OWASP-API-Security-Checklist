# OWASP API Security Checklist – Testing & Design Approach

This repository demonstrates how we classified and tested the **OWASP API Security Checklist** using **Robot Framework** and supporting security tools.

---

## 📌 Classification Overview

We classified the OWASP API Security Checklist into **two main categories**:

### 1️⃣ API by Design
Controls that should be **considered early**, during API design and architecture, at the **start of the project**.

### 2️⃣ Testing
Controls that are the **primary responsibility of the testing team**, validated through automation and security testing.

---

## 🤖 How We Tested Using Robot Framework

### 🔹 Input Validation
We validate how APIs handle incorrect or malicious input.

**Approach:**
- Call the API using **invalid data**:
  - Wrong data types
  - Invalid length
  - Incorrect format
- Assert:
  - HTTP status code = `400`
  - Error message = `Bad Request`

---

### 🔹 Security Access Control
We verify that unauthorized users cannot perform restricted actions.

**Approach:**
- Call the API using a user **without required permissions**
- Assert:
  - HTTP status code = `403`
  - Error message = `Forbidden`

---

### 🔹 Security Headers
We ensure required security headers are properly returned.

**Approach:**
- Call the API
- Validate response headers:
  - Presence of all required security headers
  - Correct and secure values for each header

---

## 🔐 HTTPS & TLS Validation

### 🌐 Online Tools
We used the following **online tools** to validate HTTPS and TLS configurations:

- SSL Labs Server Test  
  https://www.ssllabs.com/ssltest
- CryptCheck  
  https://cryptcheck.fr/
- CypherCraft  
  https://www.cyphercraft.io/
- Hardenize  
  https://www.hardenize.com/
- ImmuniWeb  
  https://www.immuniweb.com/ssl/
- Observatory by Mozilla  
  https://observatory.mozilla.org/
- SSL Configuration Generator  
  https://ssl-config.mozilla.org/

---

### 🖥️ Offline Tools
We also used **offline and CLI-based tools**:

- O-Saft – OWASP SSL Advanced Forensic Tool  
  https://wiki.owasp.org/index.php/O-Saft
- CipherScan  
  https://github.com/mozilla/cipherscan
- CryptoLyzer  
  https://gitlab.com/coroner/cryptolyzer
- SSLScan  
  https://github.com/rbsec/sslscan
- SSLyze  
  https://github.com/nabla-c0d3/sslyze
- testssl.sh  
  https://testssl.sh/
- tls-scan  
  https://github.com/prbinu/tls-scan

---

## 🔑 JWT (JSON Web Tokens)

### 🧰 Tools & References
We used the following resources to test and validate JWT security:

- OpenTelemetry  
  https://opentelemetry.io/
- JWT Introduction  
  https://jwt.io/introduction
- OWASP ZAP JWT Scanner  
  https://www.zaproxy.org/blog/2020-09-03-zap-jwt-scanner/
- MyJWT Documentation  
  https://myjwt.readthedocs.io/en/latest/
- MyJWT GitHub  
  https://github.com/mBouamama/MyJWT

---

## 📊 Sample Results

<img width="924" height="642" alt="image" src="https://github.com/user-attachments/assets/05925f22-707c-496f-96a2-0914412d8cdb" />
<img width="924" height="642" alt="image" src="https://github.com/user-attachments/assets/ff980665-cd1a-4b8c-8393-96b37279ddb6" />
<img width="924" height="642" alt="image" src="https://github.com/user-attachments/assets/1d770d65-3242-4519-aabf-aed53e5adcf5" />
<img width="924" height="642" alt="image" src="https://github.com/user-attachments/assets/45716264-caec-423c-8881-85c0297063c6" />
<img width="924" height="642" alt="image" src="https://github.com/user-attachments/assets/e97b79a3-e760-4220-a584-c08483d24c21" />




Below are examples of results obtained during testing:
- Input validation failures
- Unauthorized access attempts
- Missing or misconfigured security headers
- TLS and certificate weaknesses
- JWT validation issues

> 📸 Screenshots can be added here to visualize findings.

---

## 📚 Full Checklist

For full details and to use the complete checklist, visit:

👉 **OWASP API Security Checklist Repository**  
https://github.com/amirasolimanahmed/OWASP-API-Security-Checklist

---

## ✅ Happy Testing!
Security is a **process**, not a one-time task.  
Automate early, test continuously, and always tie controls back to risk. :)

---
