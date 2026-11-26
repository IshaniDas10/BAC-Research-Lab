# 04 — Identification, Authentication and Authorization

## 1. Identification

Identification is the process through which a user claims an identity to a system.

It answers:

**Who are you claiming to be?**

Examples:
- Username
- Email address
- Employee ID
- User ID

Identification alone does not prove the user's identity.

## 2. Authentication

Authentication verifies the identity claimed by a user.

It answers:

**Can you prove who you claim to be?**

Common methods:
- Passwords
- OTP
- Multi-factor authentication (MFA)
- Biometrics
- Digital certificates

## 3. Authorization

Authorization determines which resources and operations an authenticated user is permitted to access.

It answers:

**What are you allowed to do?**

Examples:
- View personal information.
- Edit owned resources.
- Access administrator functions.
- Perform authorized transactions.

## 4. Comparison

| Stage | Purpose | Example |
|---|---|---|
| Identification | Claims identity | Enter username |
| Authentication | Verifies identity | Enter password |
| Authorization | Checks permissions | Access admin dashboard |

## 5. Working Process

1. User provides an identity.
2. System receives the identification information.
3. System authenticates the user.
4. System determines the user's permissions.
5. System allows or denies the requested action.

## 6. AAA Security Framework

AAA stands for:

### Authentication
Verifies the identity of a user or system.

### Authorization
Determines which operations and resources the authenticated subject can access.

### Accounting
Records and tracks user activities.

Examples:
- Login history.
- Access logs.
- Administrative activity.
- Resource usage.
- Security audit records.

## 7. Relationship with Broken Access Control

Broken Access Control occurs when authorization policies are not correctly enforced.

Example:
- User A identifies themselves.
- User A successfully authenticates.
- User A requests User B's private data.
- The server fails to verify ownership.
- User A receives unauthorized information.

The identification and authentication stages succeeded, but authorization failed.

## 8. Key Takeaway

**Identification = Claim an identity.**

**Authentication = Prove the identity.**

**Authorization = Receive permitted access.**

**Accounting = Record the activity.**

Authorization is the central focus of Broken Access Control testing.

---

**Status:** Completed — Fundamentals, Topic 04  
**Category:** Access Control Fundamentals
