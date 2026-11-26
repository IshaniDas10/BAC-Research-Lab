# 03 — Authentication vs Authorization

## 1. Authentication

Authentication is the process of verifying the identity of a user, device, or system.

It answers:

**Who are you?**

Common authentication methods:
- Username and password.
- Multi-factor authentication (MFA).
- One-time passwords (OTP).
- Biometrics.
- API keys.
- Digital certificates.
- Authentication tokens.

## 2. Authorization

Authorization is the process of determining whether an authenticated subject has permission to access a resource or perform an action.

It answers:

**What are you allowed to do?**

Examples:
- A normal user can view their own profile.
- An administrator can manage user accounts.
- A customer cannot access another customer's orders.
- A manager can approve requests assigned to them.

## 3. Key Differences

| Authentication | Authorization |
|---|---|
| Verifies identity | Verifies permissions |
| Answers who you are | Answers what you can do |
| Usually occurs during login | Must be checked on protected operations |
| Uses credentials and identity factors | Uses roles, permissions, policies and context |
| Failure commonly results in 401 | Denial commonly results in 403 |

## 4. Practical Scenario

Consider an online banking application.

1. User A enters valid login credentials.
2. The server authenticates User A.
3. User A requests access to an account.
4. The server checks whether User A owns that account.
5. Access is granted only if the authorization policy permits it.

A successful login does not grant permission to access every account.

## 5. Technical Demonstration

### Vulnerable Example

```python
if token_is_valid(token):
    return get_all_users()
```

**Problem:** The application verifies identity but does not check whether the user is authorized to retrieve all user records.

### Secure Example

```python
if not token_is_valid(token):
    raise HTTPException(status_code=401)

if current_user.role != "admin":
    raise HTTPException(status_code=403)

return get_all_users()
```

**Fix:** The application checks both authentication and authorization.

## 6. Important HTTP Status Codes

| Status | Meaning |
|---|---|
| 200 | Successful request |
| 401 | Authentication required or failed |
| 403 | Access forbidden |
| 404 | Resource not found |

A 404 response can also be deliberately used to conceal the existence of a restricted resource.

## 7. Security Relevance

A system may have strong authentication but weak authorization.

For example:
- Users have strong passwords.
- MFA is enabled.
- Sessions are securely managed.
- However, users can access other users' records by changing object identifiers.

The application can still have a serious Broken Access Control vulnerability.

## 8. Pentesting Perspective

A penetration tester should:
1. Verify authentication requirements.
2. Identify different user roles.
3. Test access to restricted endpoints.
4. Compare permissions between accounts.
5. Test object ownership.
6. Verify that authorization is enforced server-side.

## 9. Key Takeaway

**Authentication establishes identity. Authorization enforces permissions.**

Both are essential, but authentication alone cannot protect an application from Broken Access Control.

---

**Status:** Completed — Fundamentals, Topic 03  
**Category:** Web Application Security
