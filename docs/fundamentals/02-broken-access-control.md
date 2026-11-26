# 02 — Broken Access Control (BAC)

## 1. Definition

Broken Access Control occurs when an application fails to properly enforce authorization policies.

It allows users to access resources, perform actions, or obtain privileges beyond what they are authorized to have.

**Key idea:** A user may be successfully authenticated but still be unauthorized to access a particular resource.

## 2. Real-World Scenario

Consider an e-commerce application.

- User A logs into their account.
- User A can view their own order.
- The order URL is `/api/orders/101`.
- User A changes the order ID to `102`.
- The server returns another user's order.

The application has failed to verify resource ownership.

This is an example of Broken Access Control.

## 3. Authentication vs Authorization

| Authentication | Authorization |
|---|---|
| Verifies user identity | Checks user permissions |
| Answers: Who are you? | Answers: What can you access? |
| Usually occurs during login | Must be enforced on protected requests |

Successful authentication does not guarantee authorization.

## 4. How Authorization Should Work

1. User sends a request.
2. Server identifies the authenticated user.
3. Server checks the requested action.
4. Server verifies resource ownership and permissions.
5. Server allows or denies access.

Authorization must be enforced on the server, not merely hidden in the frontend.

## 5. Common Types of BAC

| Type | Explanation |
|---|---|
| Horizontal Privilege Escalation | Accessing another user's resources |
| Vertical Privilege Escalation | Gaining access to higher-privilege functions |
| IDOR | Accessing objects by manipulating identifiers without proper authorization |
| BOLA | API fails to enforce object-level authorization |
| Forced Browsing | Directly accessing restricted endpoints |
| Missing Authorization | Server does not check permissions |
| Mass Assignment | Modifying protected fields through automatic object binding |

## 6. Technical Demonstration

### Vulnerable Python Example

```python
@app.get("/api/orders/{order_id}")
def get_order(order_id: int):
    return database.get_order(order_id)
```

**Problem:** The server retrieves an order without checking whether the authenticated user owns it.

### Secure Python Example

```python
@app.get("/api/orders/{order_id}")
def get_order(order_id: int, current_user = Depends(get_current_user)):
    order = database.get_order(order_id)

    if order.user_id != current_user.id:
        raise HTTPException(status_code=403)

    return order
```

**Fix:** The server verifies ownership before returning the order.

## 7. Root Causes

- Missing server-side authorization checks.
- Incorrect role or permission validation.
- Trusting user-controlled identifiers.
- Relying on frontend restrictions.
- Inadequate object ownership validation.
- Incorrect permission inheritance.
- Inconsistent authorization across endpoints.

## 8. Pentesting Methodology

1. Identify application roles.
2. Create or obtain authorized test accounts.
3. Map endpoints and resources.
4. Identify ownership relationships.
5. Test horizontal access between accounts.
6. Test vertical access between roles.
7. Test direct access to restricted endpoints.
8. Compare expected and actual authorization behavior.
9. Record reproducible evidence.
10. Verify fixes.

## 9. Security Impact

- Unauthorized data exposure.
- Account compromise or modification.
- Privilege escalation.
- Unauthorized financial transactions.
- Administrative function abuse.
- Cross-tenant data exposure.

## 10. Prevention

- Enforce authorization on every protected server-side request.
- Apply least privilege.
- Deny access by default.
- Verify resource ownership.
- Validate roles and permissions.
- Use centralized authorization logic.
- Test authorization across all roles and endpoints.
- Log and monitor suspicious access attempts.

## 11. Connection to AuthZ-Sentinel

AuthZ-Sentinel can help automate:

- Horizontal and vertical access testing.
- Object ownership verification.
- Missing authorization detection.
- Endpoint permission comparison.
- Evidence collection and vulnerability reporting.
- Authorization regression testing.

## 12. Key Takeaway

**Broken Access Control is an authorization failure, not necessarily an authentication failure.**

The application must verify not only who the user is, but also whether that user is permitted to perform the requested action on the requested resource.

---

**Status:** Completed — Fundamentals, Topic 02  
**Category:** OWASP A01 — Broken Access Control
