# 06 — Subjects, Objects, Resources, Permissions and Actions

## 1. Subject

A subject is an entity that requests access to a resource.

Examples:
- Normal user.
- Administrator.
- Employee.
- API client.
- Application or service.

Example: User A requests access to an order.

User A is the subject.

## 2. Object

An object is the specific entity on which an operation is performed.

Examples:
- User profile.
- Bank account.
- Order.
- Document.
- Database record.

Example: Order #101 is the object being requested.

## 3. Resource

A resource is a protected application component, data item or functionality that a subject attempts to access.

Examples:
- `/api/users/10`
- `/api/orders/101`
- `/admin/dashboard`
- Private documents.
- Database records.

Object and resource are often used interchangeably in access-control discussions. An object generally refers to a specific entity, while resource can describe the broader protected item or functionality.

## 4. Action

An action is the operation a subject wants to perform on a resource.

Common CRUD operations:

| Action | Meaning | Typical HTTP method |
|---|---|---|
| Create | Create a resource | POST |
| Read | Retrieve information | GET |
| Update | Modify information | PUT/PATCH |
| Delete | Remove a resource | DELETE |

HTTP methods do not always map perfectly to CRUD operations; applications must enforce permissions based on actual behavior.

## 5. Permission

A permission defines whether a subject is allowed to perform a particular action on a resource.

Examples:
- User A can read their own profile.
- User A can update their own profile.
- User A cannot delete another user's account.
- An administrator can manage user accounts.

## 6. Authorization Relationship

An authorization decision can be represented using:

**Subject + Action + Resource + Context = Access Decision**

Example:

| Component | Value |
|---|---|
| Subject | User A |
| Role | Customer |
| Resource | Order #101 |
| Owner | User A |
| Action | Read |
| Expected decision | Allow |

If User B requests User A's order, the expected decision is Deny, unless an explicitly defined policy grants User B access.

## 7. Technical Demonstration

```python
def can_access(user, order, action):
    if order.user_id != user.id:
        return False

    if action not in ["read", "update"]:
        return False

    return True
```

This simplified example checks resource ownership and permitted actions.

## 8. Connection to Broken Access Control

| Vulnerability | Authorization failure |
|---|---|
| IDOR/BOLA | Unauthorized object access |
| Privilege escalation | Unauthorized privileges |
| Forced browsing | Unauthorized resource access |
| Mass assignment | Unauthorized property modification |
| Function-level authorization failure | Unauthorized function execution |

## 9. Connection to AuthZ-Sentinel

AuthZ-Sentinel can model authorization requests using:
- Subject identity.
- Subject role.
- Resource identifier.
- Resource owner.
- Requested action.
- Expected authorization decision.
- Actual authorization decision.

This information forms the foundation of its permission matrix and ownership-aware testing engine.

## 10. Key Takeaway

**Subject = Who requests access.**

**Object = What specific entity is being accessed.**

**Resource = What the application protects.**

**Action = What operation is requested.**

**Permission = Whether that operation is allowed.**

---

**Status:** Completed — Fundamentals, Topic 06  
**Category:** Access Control Fundamentals
