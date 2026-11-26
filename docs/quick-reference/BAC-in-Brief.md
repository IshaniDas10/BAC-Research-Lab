# 🏰 Broken Access Control (BAC) in Brief
### Everything You Need to Know in One Minute

Imagine a kingdom with thousands of citizens, rooms, treasure chests and guards.

Every citizen has an identity, every room has permissions, and every guard must ensure that people access only what they're allowed to.

**Broken Access Control happens when the guards let someone do something they shouldn't be allowed to do.**

## 1. The Fundamentals — Know the Kingdom
- **Identification:** Who are you?
- **Authentication:** Prove who you are.
- **Authorization:** What are you allowed to do?
- **Session Management:** Remember who is making requests.
- **Least Privilege:** Give only the permissions necessary.
- **Deny by Default:** If access isn't explicitly allowed, deny it.
- **Server-side Enforcement:** The actual server must check permissions.

## 2. Access Control Models — Who Gets the Keys?
- **DAC:** Resource owners decide who gets access.
- **MAC:** Access follows mandatory security classifications.
- **RBAC:** Permissions depend on roles.
- **ABAC:** Permissions depend on attributes.
- **ReBAC:** Permissions depend on relationships.
- **ACL:** Each resource has a list of permitted users.
- **Capability-Based:** Possessing a valid capability grants specific access.

## 3. Common BAC Vulnerabilities — How Guards Fail
- Missing or incorrect authorization checks.
- Client-side-only access restrictions.
- Horizontal and vertical privilege escalation.
- Unauthorized object, function and field access.
- Unprotected endpoints and administrative functions.
- Inconsistent or excessive default permissions.

## 4. IDOR and BOLA — Opening Someone Else's Treasure Chest
- **IDOR:** Manipulate an object reference to access another user's resource.
- **BOLA:** An API fails to enforce authorization for the requested object.
- Test user IDs, account IDs, order IDs, document IDs and other resource identifiers.
- Check unauthorized read, write, delete and bulk access.

## 5. Privilege Escalation — Gaining Unauthorized Power
- **Horizontal:** Access another user's resources.
- **Vertical:** Gain higher privileges, such as administrator access.
- Role manipulation, role confusion, admin bypass and function-level authorization failures.

## 6. Forced Browsing and Endpoint Bypass — Finding Secret Entrances
- Direct URL access and hidden endpoints.
- Administrative API discovery.
- HTTP method tampering: GET, POST, PUT, PATCH, DELETE.
- Alternate routes and URL rewriting.
- Proxy-related headers and incorrectly trusted client headers.

## 7. Parameter Tampering and Mass Assignment — Changing the Application Form
- Manipulate query, path, body and cookie parameters.
- Tamper with user IDs, roles, `isAdmin` and other sensitive fields.
- **Mass Assignment:** The application accepts fields users should not control.
- Protect server-controlled properties using explicit allowlists.

## 8. JWT, Cookies and Sessions — Protecting Identity Passes
- Cookies, session IDs, session fixation and hijacking.
- JWT headers, payloads, signatures and claims.
- Signature validation, expiry, refresh tokens and revocation.
- Password reset, account recovery and OAuth 2.0.
- Secure, HttpOnly and SameSite cookie attributes.

## 9. CSRF, CORS and Browser Security — Protecting the Kingdom's Borders
- CSRF, CSRF tokens and unwanted authenticated actions.
- Same-Origin Policy and CORS.
- Origin validation, preflight and credentialed requests.
- Clickjacking, frame protection and CSP.

## 10. Files and Paths — Protecting the Royal Library
- Path and directory traversal.
- Relative paths, absolute paths and `../`.
- File upload/download authorization.
- File ownership, permissions, symlinks and directory listing.
- File IDOR, arbitrary read and arbitrary write.

## 11. SSRF and Server Boundaries — Protecting Internal Rooms
- Server-Side Request Forgery (SSRF).
- Internal IPs, loopback and cloud metadata.
- URL and DNS validation, redirects and DNS rebinding.
- Egress restrictions, protocol restrictions and confused deputy problems.

## 12. Business Logic and Advanced Authorization — Protecting Kingdom Rules
- Workflow and approval bypass.
- Transaction and state-transition authorization.
- Multi-tenant isolation and cross-tenant access.
- Field/property-level authorization.
- GraphQL, WebSocket and microservice authorization.
- Race conditions, TOCTOU, delegated permissions and revocation.

## 13. Information Disclosure and Misconfiguration — Secrets Left Unlocked
- Sensitive information and PII exposure.
- API overexposure and private profile exposure.
- Source code, configuration, debug and backup exposure.
- Public resources, default permissions and ownership misconfiguration.

## 14. BAC Pentesting — Become the Kingdom's Investigator
- Reconnaissance and application mapping.
- Endpoint inventory and role identification.
- Anonymous, User A, User B and Admin testing.
- Permission matrices and access comparisons.
- IDOR, privilege escalation, forced browsing and parameter testing.
- Evidence collection, validation, false-positive checks and retesting.

## 15. Prevention — Build Better Guards
- Authorize every request on the server.
- Apply least privilege and deny by default.
- Verify ownership, tenant, object, function and field permissions.
- Use centralized authorization policies.
- Secure sessions, JWTs, files, workflows and internal services.
- Audit, monitor and continuously regression-test authorization.

## 16. Tools and Standards — The Investigator's Toolkit
- **Burp Suite:** Proxy, Repeater, Intruder and Autorize.
- **OWASP ZAP:** Web security testing.
- **Postman / HTTPX:** API testing.
- **Playwright:** Browser automation.
- **JWT.io:** JWT inspection.
- **Wireshark:** Network traffic analysis.
- **OWASP WSTG:** Web security testing guidance.
- **OWASP Top 10 A01:2025:** Broken Access Control.
- **OWASP API Security Top 10:** BOLA, BFLA and object-property authorization risks.
- **CWE:** Common software weakness classifications.
- **SARIF:** Standardized security finding format.

---

## The Entire Concept in 10 Seconds

1. Authentication = Who are you?
2. Authorization = What can you do?
3. BAC = You can do something you shouldn't.
4. IDOR/BOLA = You access someone else's resource.
5. Horizontal escalation = You access another user's level.
6. Vertical escalation = You gain higher privileges.
7. Forced browsing = You find an unprotected entrance.
8. Parameter tampering = You manipulate submitted information.
9. Business logic bypass = You break the application's rules.
10. Prevention = Verify authorization at every protected action.

**The ultimate pentester question:**

*Who is making this request, what resource or action are they requesting, and does the server actually verify that they are allowed to do it?*

> Important: CSRF, CORS, SSRF, JWT flaws and path traversal are related security topics, but they are not automatically Broken Access Control vulnerabilities. Their classification depends on the actual weakness and impact.

**Learn it. Test it. Prove it. Fix it.**
