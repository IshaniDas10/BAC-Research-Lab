# 07 — Trust Boundaries

## 1. Definition

A trust boundary is a point where data, requests or control move between security domains with different levels of trust.

Information crossing a trust boundary must be treated according to an explicit security policy rather than blindly trusted.

## 2. Example

Consider an e-commerce application.

A user sends:

```http
POST /api/update-profile
Content-Type: application/json
```

```json
{
  "name": "Ishani",
  "role": "admin"
}
```

The application must not trust the submitted role value.

The backend must independently determine whether the authenticated user is authorized to modify that field.

## 3. Common Trust Boundaries

- Browser to backend.
- Internet to internal network.
- API gateway to application service.
- One microservice to another.
- Application to database.
- Tenant A to Tenant B.
- Normal user functionality to administrative functionality.
- Application to third-party service.

## 4. Client-Side vs Server-Side Trust

Client-side components include:
- HTML.
- JavaScript.
- Browser storage.
- Hidden form fields.
- User-controlled HTTP requests.

Users can inspect and manipulate these components.

Therefore, client-side restrictions cannot be treated as authorization enforcement.

The backend must independently validate permissions before performing protected operations.

## 5. Example of an Authorization Failure

Vulnerable frontend logic:

```javascript
if (user.role === "admin") {
    showAdminPanel();
}
```

Hiding an admin panel does not protect the corresponding backend API.

An attacker may send requests directly to the endpoint using an HTTP client or proxy.

If the backend does not enforce authorization, restricted functionality may be exposed.

## 6. Trust vs Validation

| Trust | Validation |
|---|---|
| Assumes information is reliable | Checks information against rules |
| May rely on user-controlled values | Uses explicit verification |
| Can create security weaknesses | Helps enforce security policies |

A submitted user ID or tenant ID must not be accepted as proof of ownership or authorization.

## 7. Pentesting Methodology

In an authorized testing environment:

1. Inspect browser requests.
2. Modify object identifiers.
3. Modify role and privilege parameters.
4. Change cookies and local storage values.
5. Send API requests directly.
6. Test endpoints without using the frontend.
7. Manipulate tenant identifiers.
8. Verify backend authorization decisions.

Useful tools:
- Burp Suite.
- OWASP ZAP.
- Browser Developer Tools.
- Postman.
- curl.

## 8. Connection to Broken Access Control

Trust-boundary failures can cause:
- IDOR/BOLA.
- Privilege escalation.
- Mass assignment.
- Forced browsing.
- Tenant isolation failures.
- Unauthorized API access.

## 9. Connection to AuthZ-Sentinel

AuthZ-Sentinel should treat user-controlled identifiers, roles, tenant IDs, object IDs and request parameters as untrusted input.

It should compare actual authorization behavior against an explicitly defined or human-confirmed policy.

## 10. Key Takeaway

**Never trust a client-side security restriction as proof of authorization.**

Every protected server-side operation must independently enforce the appropriate authorization policy.

---

**Status:** Completed — Fundamentals, Topic 07  
**Category:** Application Security Architecture
