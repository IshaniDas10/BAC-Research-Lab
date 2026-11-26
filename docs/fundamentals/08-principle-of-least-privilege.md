# 08 — Principle of Least Privilege (PoLP)

## 1. Definition

The Principle of Least Privilege (PoLP) states that every user, application, process or system should receive only the minimum permissions required to perform its assigned tasks.

Its objective is to reduce unnecessary access and limit the potential damage caused by compromised accounts or security vulnerabilities.

## 2. Example

Consider a college management system.

| Role | Permissions |
|---|---|
| Student | View marks, download notes |
| Faculty | Upload notes, update assigned students' marks |
| Administrator | Manage users, manage system settings |

A student does not need administrator permissions to perform academic activities.

Granting unnecessary permissions violates least privilege.

## 3. Why Least Privilege Matters

- Reduces the attack surface.
- Limits unauthorized access.
- Reduces the impact of compromised accounts.
- Prevents unnecessary administrative privileges.
- Limits accidental or malicious modifications.
- Supports effective access-control management.

## 4. Relationship with Privilege Escalation

Excessive permissions can create opportunities for privilege escalation.

### Horizontal Privilege Escalation

A user accesses resources belonging to another user with similar privileges.

Example: Customer A accesses Customer B's orders.

### Vertical Privilege Escalation

A lower-privileged user gains access to higher-privileged functions.

Example: A student accesses administrator functionality.

## 5. Technical Demonstration

### Vulnerable Example

```python
def get_permissions(user):
    return ["read", "write", "delete", "admin"]
```

Every user receives unrestricted permissions.

### Better Example

```python
ROLE_PERMISSIONS = {
    "student": ["read_notes", "view_marks"],
    "faculty": ["read_notes", "update_marks"],
    "admin": ["read", "write", "delete", "manage_users"]
}

def get_permissions(user):
    return ROLE_PERMISSIONS.get(user.role, [])
```

The application assigns permissions according to the user's role.

Resource ownership, scope and contextual policies must also be checked where applicable.

## 6. Least Privilege Beyond Users

PoLP also applies to:
- Application accounts.
- Database accounts.
- API keys.
- Microservices.
- Operating system processes.
- Cloud service identities.

Example: A reporting service should not receive permission to delete production database records if it only needs read access.

## 7. Pentesting Methodology

In an authorized testing environment:

1. Identify application roles.
2. Determine expected permissions for each role.
3. Test lower-privileged accounts against restricted endpoints.
4. Test unauthorized administrative actions.
5. Check whether API keys have excessive permissions.
6. Compare actual permissions with the intended policy.
7. Document excess privileges and their impact.

## 8. Connection to Broken Access Control

Violations of least privilege may result in:
- Horizontal privilege escalation.
- Vertical privilege escalation.
- Unauthorized resource access.
- Unauthorized modification.
- Excessive API permissions.

## 9. Connection to AuthZ-Sentinel

AuthZ-Sentinel can use least privilege to define expected authorization policies.

It can compare:
- Subject and role.
- Requested resource.
- Requested action.
- Expected decision.
- Actual decision.

Unexpectedly granted permissions can become security findings.

## 10. Key Takeaway

**Give every user and system only the permissions necessary to perform its assigned responsibilities.**

Reducing unnecessary privileges reduces the potential impact of Broken Access Control.

---

**Status:** Completed — Fundamentals, Topic 08  
**Category:** Access Control Principles
