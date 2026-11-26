# 01 — What Is Access Control?

## 1. Definition

Access control is a security mechanism that determines **who can access a resource, what action they can perform, and under what conditions**.

It prevents unauthorized users from accessing data, modifying information, or performing restricted operations.

## 2. Key Components

| Component | Meaning |
|---|---|
| Subject | Entity requesting access |
| Object | Resource being accessed |
| Action | Operation requested |
| Permission | Allowed operation |
| Policy | Rules determining access |
| Resource | Protected data or functionality |

## 3. Example

Consider an online banking application.

- Subject: User A
- Object: User A's bank account
- Action: View account balance
- Permission: View own account
- Policy: Users can only view accounts they own.

If User A attempts to access User B's account without permission, the application must deny the request.

## 4. Authentication vs Authorization

- **Authentication:** Verifies who the user is.
- **Authorization:** Determines what the authenticated user is allowed to do.

A successful login does not automatically grant access to every resource.

## 5. Why Access Control Matters

- Protects sensitive information.
- Prevents unauthorized modifications.
- Restricts privileged operations.
- Maintains user and tenant isolation.
- Reduces the risk of Broken Access Control.

## 6. Connection to Broken Access Control

Broken Access Control occurs when an application fails to properly enforce its access-control policies.

For example, User A changes an account ID in an API request and successfully views User B's private account information.

This is an example of an authorization failure.

## 7. Key Takeaway

**Authentication answers: Who are you?**

**Authorization answers: What are you allowed to do?**

Access control ensures that every user can access only the resources and perform only the actions permitted by the application's security policies.

---

*Status: Learning note 01 — Fundamentals*
