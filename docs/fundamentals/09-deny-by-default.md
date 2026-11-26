# 09 — Deny by Default (Fail-Secure Principle)

## 1. Definition

Deny by Default is an access-control principle stating that access should be denied unless an explicit authorization policy grants permission.

If permission is missing, invalid or unknown, the application should not automatically grant access.

## 2. Example

Consider a college management portal.

A student attempts to access the administrator dashboard.

The application checks whether the student has administrator permission.

If the permission is absent, the application denies access.

## 3. Default Allow vs Deny by Default

| Default Allow | Deny by Default |
|---|---|
| Grants access unless explicitly blocked | Denies access unless explicitly permitted |
| Missing rules may expose resources | Missing rules do not grant access |
| Increases unauthorized access risk | Provides a safer authorization baseline |

## 4. Authorization Flow

1. Receive the request.
2. Identify the subject.
3. Determine the requested action and resource.
4. Evaluate the applicable authorization policy.
5. Allow only if permission is explicitly established.
6. Otherwise deny the operation.

## 5. Technical Demonstration

### Vulnerable Example

```python
def authorize(user, resource):
    if user.role == "blocked":
        return False

    return True
```

This implementation grants access to every user except those explicitly blocked.

Unknown roles may also receive access.

### Secure Example

```python
def authorize(user, resource):
    if user is None:
        return False

    if not user.has_permission(resource):
        return False

    return True
```

This simplified implementation requires an explicit permission.

Production systems must also evaluate the requested action, ownership and relevant contextual policies.

## 6. Fail-Secure Principle

Fail-Secure means that when a security mechanism encounters an unexpected failure, it should preserve the security of the protected resource.

Example:

If an authorization service becomes unavailable, a protected administrative operation should not automatically be allowed.

The operation should be denied until authorization can be evaluated safely.

## 7. Common Implementation Mistakes

- Granting permissions to unknown roles.
- Treating missing permissions as allowed.
- Allowing access when authorization services fail.
- Creating endpoints without authorization rules.
- Using overly broad wildcard permissions.
- Assuming authentication grants unrestricted access.

## 8. Relationship with Broken Access Control

Default-allow behavior can expose restricted resources when authorization checks are missing.

Deny-by-default reduces this risk by requiring explicit permission before protected operations are performed.

## 9. Pentesting Methodology

In an authorized environment:

1. Identify endpoints with unclear authorization policies.
2. Test anonymous access.
3. Test normal user access.
4. Test unknown or restricted roles.
5. Test missing and invalid authorization information.
6. Verify secure behavior when authorization dependencies fail.

Document any protected operation that succeeds without an appropriate authorization decision.

## 10. Connection to AuthZ-Sentinel

AuthZ-Sentinel can use deny-by-default as its baseline authorization policy.

Example:

| Subject | Resource | Action | Expected |
|---|---|---|---|
| Anonymous | Admin dashboard | Read | Deny |
| Student | Own marks | Read | Allow |
| Student | Admin dashboard | Read | Deny |
| Faculty | Assigned marks | Update | Allow |
| Unknown role | User management | Delete | Deny |

An endpoint without a confirmed policy should be flagged for review rather than automatically classified as a confirmed vulnerability.

## 11. Key Takeaway

**No explicit permission means no access.**

Deny by Default reduces the risk of accidental permission grants and is a fundamental principle for preventing Broken Access Control.

---

**Status:** Completed — Fundamentals, Topic 09  
**Category:** Access Control Principles
