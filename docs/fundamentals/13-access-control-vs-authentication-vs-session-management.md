# Access Control vs Authentication vs Session Management

## 1. Authentication

Authentication verifies the identity of a user.

Example:

A user enters a username, password, and possibly an OTP.

The application verifies the credentials and establishes the user's identity.

Common authentication methods:

- Password-based authentication
- OTP
- Multi-factor authentication (MFA)
- Biometrics
- Certificate-based authentication
- Passkeys

**Main question: Who are you?**

## 2. Session Management

Session management maintains authenticated state across multiple HTTP requests.

HTTP is stateless by default. Therefore, the server needs a mechanism to associate multiple requests with the same authenticated user.

### How a session works

1. User submits login credentials.
2. Server verifies the credentials.
3. Server creates a session.
4. Server sends a session identifier to the browser.
5. Browser stores the session cookie.
6. Browser sends the cookie with subsequent requests.
7. Server validates the session and identifies the associated user.

Example response:

```http
HTTP/1.1 200 OK
Set-Cookie: session_id=abc123xyz;
```

Subsequent request:

```http
GET /dashboard HTTP/1.1
Cookie: session_id=abc123xyz
```

A valid session identifier must be protected because an attacker possessing it may be able to act as the authenticated user.

## 3. Access Control

Access control determines which resources and operations a user is permitted to access.

Example:

| Action | Normal User | Administrator |
|---|---|---|
| View own profile | Yes | Yes |
| Edit own profile | Yes | Yes |
| Delete users | No | Yes |
| Change user roles | No | Yes |

**Main question: What are you allowed to do?**

## 4. Key Differences

| Concept | Main Question | Responsibility |
|---|---|---|
| Authentication | Who are you? | Verifies identity |
| Session Management | How does the application remember you? | Maintains authenticated state |
| Access Control | What can you do? | Enforces permissions |

Memory trick:

- Authentication = Identity.
- Session Management = Continuity.
- Access Control = Permission.

## 5. Common Security Failures

### Authentication Failures

- Weak passwords.
- Authentication bypass.
- Weak MFA implementation.
- Credential stuffing.
- Broken login logic.

### Session Management Failures

- Session fixation.
- Session hijacking.
- Predictable session identifiers.
- Missing session expiration.
- Session not invalidated after logout.
- Insecure cookie configuration.

### Access Control Failures

- IDOR / BOLA.
- Privilege escalation.
- Forced browsing.
- Unauthorized administrative actions.
- Cross-tenant data access.

These categories may overlap, but their underlying root causes should be identified separately.

## 6. Example: Authentication Succeeds but Authorization Fails

User A is authenticated and has a valid session.

User A sends:

```http
GET /api/accounts/2002
Cookie: session_id=USER_A_SESSION
```

Account 2002 belongs to User B.

The server validates User A's session but fails to check whether User A can access account 2002.

It returns:

```http
HTTP/1.1 200 OK

{
  "account_id": 2002,
  "owner": "User B",
  "balance": 50000
}
```

This is a Broken Access Control vulnerability.

The three stages:

1. Authentication succeeded.
2. Session validation succeeded.
3. Object-level authorization failed.

**A valid session does not mean unlimited access.**

## 7. Secure Request Flow

1. Receive HTTP request.
2. Authenticate the user.
3. Validate the session or token.
4. Perform authorization checks.
5. Execute the permitted operation.

If authorization fails, deny the operation.

Depending on the application, the server may return 403 Forbidden or 404 Not Found.

## 8. Penetration Testing Checklist

| Test | Security Area |
|---|---|
| Invalid login credentials | Authentication |
| Reuse an old session | Session validation |
| Test session expiration | Session management |
| Access after logout | Session invalidation |
| Change user ID in request | Object authorization |
| Access admin endpoint as normal user | Function authorization |
| Access another tenant's resource | Tenant isolation |

Tools:

- Burp Suite
- OWASP ZAP
- Postman
- Browser Developer Tools
- cURL

Only test applications you own or have explicit permission to assess.

## 9. AuthZ-Sentinel Connection

AuthZ-Sentinel should separate these three responsibilities.

| Layer | Responsibility |
|---|---|
| Authentication | Establish test identities |
| Session Management | Maintain test sessions and tokens |
| Access Control | Test permissions across identities and resources |

Example workflow:

1. Login manager creates User A and User B sessions.
2. Session manager maintains their cookies and tokens.
3. Authorization engine tests whether User A can access User B's objects.

This separation improves architecture and finding accuracy.

## 10. Interview Questions

**Q1. What is authentication?**

The process of verifying a user's identity.

**Q2. What is session management?**

The process of maintaining authenticated state across multiple HTTP requests.

**Q3. What is access control?**

The mechanism that determines and enforces what a user can access or perform.

**Q4. Can an authenticated user still exploit Broken Access Control?**

Yes. Authentication does not guarantee permission to access every resource.

**Q5. What should happen after logout?**

The server should invalidate the session or otherwise prevent the logged-out session from being reused.

**Q6. What is the relationship between sessions and cookies?**

Cookies are a common mechanism for carrying session identifiers between the browser and server.

## Key Takeaway

Authentication verifies identity.

Session management maintains authenticated state.

Access control enforces permissions.

All three are necessary for a secure application.
