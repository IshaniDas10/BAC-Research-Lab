# Authorization Enforcement Architecture — PEP, PDP, PAP and PIP

## 1. Introduction

Modern applications may use a structured authorization architecture to separate policy enforcement, policy decisions, policy administration and policy information.

Four important components:

1. PEP — Policy Enforcement Point
2. PDP — Policy Decision Point
3. PAP — Policy Administration Point
4. PIP — Policy Information Point

These concepts are commonly associated with policy-based access-control architectures and XACML.

## 2. PEP — Policy Enforcement Point

PEP stands for Policy Enforcement Point.

**Responsibility:** Enforce authorization decisions.

It acts as a security checkpoint between a user request and a protected resource.

Functions:

- Intercepts access requests.
- Sends authorization information for evaluation.
- Receives an allow or deny decision.
- Allows or blocks the requested operation.

Example:

```python
def access_resource(user, resource):
    decision = authorization_service.check(
        user=user,
        action="read",
        resource=resource
    )

    if not decision:
        return {"error": "Forbidden"}, 403

    return get_resource(resource)
```

**Memory trick: PEP = Enforces the decision.**

## 3. PDP — Policy Decision Point

PDP stands for Policy Decision Point.

**Responsibility:** Evaluate authorization policies and decide whether access should be allowed or denied.

Example policy:

```text
IF user.role == "admin"
THEN allow access to admin reports
ELSE deny access
```

Functions:

- Receives authorization requests.
- Evaluates applicable policies.
- Determines allow or deny.
- Returns the authorization decision.

**Memory trick: PDP = Makes the decision.**

## 4. PAP — Policy Administration Point

PAP stands for Policy Administration Point.

**Responsibility:** Create, manage, update and distribute authorization policies.

Example:

```text
Policy 1:
Students can view their own grades.

Policy 2:
Teachers can view grades of students in their assigned classes.

Policy 3:
Administrators can manage user accounts.
```

Functions:

- Creates authorization policies.
- Updates existing policies.
- Removes outdated policies.
- Manages policy versions.
- Makes policies available to the authorization system.

**Memory trick: PAP = Manages the policies.**

## 5. PIP — Policy Information Point

PIP stands for Policy Information Point.

**Responsibility:** Provide information required to evaluate authorization decisions.

Example:

A policy says teachers can view grades only for students in their assigned classes.

The authorization system may need:

- Teacher ID.
- Teacher's assigned classes.
- Student's class.
- Resource ownership information.

PIP supplies the relevant information.

Possible information sources:

- User databases.
- Role directories.
- Organization records.
- Resource ownership records.
- Tenant information.
- Environmental context.

**Memory trick: PIP = Provides policy-related information.**

## 6. How All Four Work Together

Example: A teacher requests access to a student's grades.

1. PEP intercepts the request.
2. PDP evaluates the authorization policy.
3. PDP obtains additional information from PIP if needed.
4. PAP manages the policy used by the authorization system.
5. PDP returns an allow or deny decision.
6. PEP enforces the decision.

| Component | Responsibility |
|---|---|
| PEP | Enforces the decision |
| PDP | Makes the decision |
| PAP | Manages policies |
| PIP | Supplies information |

## 7. Multi-Tenant Example

Imagine a SaaS application with two companies:

- Company A
- Company B

The policy states:

```text
Users can access documents
only within their own tenant.
```

A user from Company A requests a document belonging to Company B.

| Component | Responsibility |
|---|---|
| PEP | Intercepts the document request |
| PDP | Evaluates whether access is permitted |
| PIP | Supplies user and document tenant information |
| PAP | Manages the tenant-access policy |

The PDP should return DENY if the user's tenant does not match the permitted tenant context.

PEP must enforce the denial.

## 8. Failure Handling

| Failure | Secure Behavior |
|---|---|
| PDP unavailable | Deny protected access |
| PIP unavailable | Deny if required information cannot be verified |
| Policy missing | Deny by default |
| Policy outdated | Apply controlled policy-update rules |
| PEP bypassed | Prevent direct access to protected operations |

Authorization systems should fail securely instead of converting errors into permission.

## 9. Connection to Broken Access Control

Without proper authorization enforcement, a user may access resources belonging to another user.

For example:

```text
User ID: 101
Document ID: 502
Document Owner: 202
Action: READ

Decision: DENY
```

A correctly designed authorization system can help prevent:

- IDOR
- BOLA
- Privilege escalation
- Unauthorized function access
- Cross-tenant access

## 10. AuthZ-Sentinel Connection

AuthZ-Sentinel can adapt these architectural responsibilities.

| Component | AuthZ-Sentinel Module |
|---|---|
| PEP | Controlled testing interceptor |
| PDP | Authorization analysis engine |
| PAP | Policy management and permission matrix |
| PIP | Identity and resource context provider |

The testing engine should compare observed application behavior against reviewed authorization policies.

AuthZ-Sentinel is a testing tool; it does not necessarily act as the target application's enforcement system.

## 11. Interview Questions

**Q1. What is PEP?**

Policy Enforcement Point. It enforces authorization decisions.

**Q2. What is PDP?**

Policy Decision Point. It evaluates policies and makes access decisions.

**Q3. What is PAP?**

Policy Administration Point. It manages authorization policies.

**Q4. What is PIP?**

Policy Information Point. It supplies information needed for authorization decisions.

**Q5. Difference between PEP and PDP?**

PEP enforces the decision; PDP makes the decision.

**Q6. What happens if the PDP becomes unavailable?**

Protected operations should fail securely instead of being automatically allowed.

**Q7. Which component manages authorization policies?**

PAP.

## Key Takeaway

- PEP = Enforces.
- PDP = Decides.
- PAP = Manages policies.
- PIP = Provides information.

Together, these components help build structured, centralized and maintainable authorization systems.
