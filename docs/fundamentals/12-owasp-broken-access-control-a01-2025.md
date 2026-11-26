# OWASP Broken Access Control — A01:2025

## 1. What is OWASP?

OWASP stands for Open Worldwide Application Security Project.

It is a nonprofit community that develops resources, standards, tools, and guidance to improve software security.

Important OWASP projects:

- OWASP Top 10
- OWASP Web Security Testing Guide
- OWASP API Security Top 10
- OWASP Juice Shop
- OWASP ZAP

## 2. What is OWASP Top 10?

OWASP Top 10 is an awareness document that highlights major categories of web application security risks.

The 2025 edition contains:

| ID | Category |
|---|---|
| A01:2025 | Broken Access Control |
| A02:2025 | Security Misconfiguration |
| A03:2025 | Software Supply Chain Failures |
| A04:2025 | Cryptographic Failures |
| A05:2025 | Injection |
| A06:2025 | Insecure Design |
| A07:2025 | Authentication Failures |
| A08:2025 | Software or Data Integrity Failures |
| A09:2025 | Security Logging and Alerting Failures |
| A10:2025 | Mishandling of Exceptional Conditions |

## 3. What is A01:2025 Broken Access Control?

Broken Access Control occurs when an application does not properly enforce restrictions on what authenticated or unauthenticated users are allowed to access or perform.

Examples:

- Accessing another user's private documents.
- A normal user accessing administrator functionality.
- Modifying another user's account.
- Accessing protected resources without authentication.
- Accessing resources belonging to another tenant.

## 4. Common Access-Control Failure Types

### Object-Level Failures

- IDOR
- BOLA
- Unauthorized object reading
- Unauthorized object modification
- Cross-user access
- Cross-tenant access

### Function-Level Failures

- Privilege escalation
- BFLA
- Forced browsing
- Unauthorized administrative actions
- Missing function-level checks

### Other Access-Control Failures

- Missing authorization checks
- Client-side-only enforcement
- Inconsistent authorization checks
- Permission misconfiguration
- Unauthorized resource access

These are useful testing groupings, not separate official OWASP A01 subcategories.

## 5. OWASP Top 10 vs OWASP API Security Top 10

| OWASP Top 10 | OWASP API Security Top 10 |
|---|---|
| Covers web application security risks broadly | Focuses on API security risks |
| A01:2025 is Broken Access Control | API1:2023 is BOLA |
| Broader application scope | API-specific scope |

Important API Security Top 10 categories:

| ID | Category |
|---|---|
| API1:2023 | Broken Object Level Authorization |
| API2:2023 | Broken Authentication |
| API3:2023 | Broken Object Property Level Authorization |
| API4:2023 | Unrestricted Resource Consumption |
| API5:2023 | Broken Function Level Authorization |
| API6:2023 | Unrestricted Access to Sensitive Business Flows |
| API7:2023 | Server Side Request Forgery |
| API8:2023 | Security Misconfiguration |
| API9:2023 | Improper Inventory Management |
| API10:2023 | Unsafe Consumption of APIs |

## 6. Vulnerability Classification

| Vulnerability | Primary category |
|---|---|
| Accessing another user's invoice | Broken Access Control |
| SQL injection | Injection |
| Weak password hashing | Cryptographic Failures |
| Missing security headers | Often Security Misconfiguration |
| XSS | Injection in OWASP Top 10:2025 |
| Unauthorized administrator access | Broken Access Control |
| SSRF | API7:2023 in API Security Top 10 |

Not every vulnerability involving a request or resource is automatically Broken Access Control. Classification should follow the actual root cause.

## 7. Example: Mapping a BOLA Finding

A normal user accesses:

```http
GET /api/invoices/502
```

The invoice belongs to another user.

The server returns private invoice information without authorization.

Classification:

1. Unauthorized action: Reading another user's invoice.
2. Root cause: Missing object-level authorization.
3. Vulnerability: BOLA / IDOR.
4. OWASP API mapping: API1:2023.
5. OWASP Top 10 mapping: A01:2025.

## 8. AuthZ-Sentinel Connection

AuthZ-Sentinel can map detected findings to OWASP categories.

Example finding:

- Finding: Cross-user invoice access.
- Endpoint: `GET /api/invoices/{id}`.
- Test identity: User A.
- Resource owner: User B.
- Root cause: Missing object-level authorization.
- Vulnerability: BOLA / IDOR.
- OWASP API mapping: API1:2023.
- OWASP Top 10 mapping: A01:2025.

The reporting engine should include:

- Finding title.
- Affected endpoint.
- HTTP method.
- Test identity.
- Resource owner.
- Expected behavior.
- Actual behavior.
- Evidence.
- Impact.
- Severity.
- Confidence.
- OWASP mapping.
- Remediation.

Severity must be based on actual impact and exploitability, not simply the OWASP category.

## 9. Interview Questions

**Q1. What is OWASP?**

An open community focused on improving software security through standards, tools, and educational resources.

**Q2. What is OWASP Top 10?**

An awareness document describing major web application security risks.

**Q3. Which OWASP category covers Broken Access Control?**

A01:2025.

**Q4. What is BOLA?**

Broken Object Level Authorization, where a user accesses an object without the required permission.

**Q5. What is BFLA?**

Broken Function Level Authorization, where a user executes a function without the required permission.

**Q6. Is IDOR identical to BOLA?**

No. IDOR describes an insecure direct object reference pattern, while BOLA describes the underlying API object-level authorization failure.

## Key Takeaway

OWASP A01:2025 covers Broken Access Control.

BOLA and BFLA are important API authorization failure categories.

Correct classification helps security testers communicate findings, prioritize remediation, and produce professional reports.
