# Server-Side vs Client-Side Access Control

## 1. Client-Side Access Control

Client-side access control refers to permission-related checks performed in the user's browser.

It controls what users can see or interact with.

Example:

```javascript
if (user.role === "admin") {
    showDeleteButton();
}
else {
    hideDeleteButton();
}
```

This only hides the button. It does not guarantee that the backend will reject an unauthorized request.

### Common examples

- Hiding admin buttons.
- Disabling restricted buttons.
- JavaScript role checks.
- Hiding HTML elements using CSS.
- Protecting frontend routes.
- Storing roles in local storage.

## 2. Server-Side Access Control

Server-side access control means the backend verifies permissions before allowing an action.

Example:

```python
@app.route("/users/<int:user_id>/delete", methods=["DELETE"])
def delete_user(user_id):
    if current_user.role != "admin":
        return {"error": "Forbidden"}, 403

    delete_user_from_database(user_id)
    return {"message": "Deleted"}, 200
```

The server independently checks whether the current user is authorized.

## 3. Key Differences

| Client-Side | Server-Side |
|---|---|
| Runs in browser | Runs on backend |
| Controls interface visibility | Controls actual permissions |
| Can be manipulated | Enforced by server |
| Improves user experience | Provides security enforcement |
| Cannot be trusted as the security boundary | Must enforce authorization |

**Important:** Client-side checks are useful for user experience, but server-side authorization is essential for security.

## 4. How Broken Access Control Occurs

Consider an application where only administrators should delete users.

The frontend hides the Delete button from normal users.

However, the backend does not check the user's role.

An attacker can send a direct request:

```http
DELETE /api/users/105 HTTP/1.1
Host: example.com
Cookie: session=USER_SESSION
```

If the backend deletes the user despite the requester lacking permission, Broken Access Control exists.

A secure backend should reject the request, commonly with:

```http
HTTP/1.1 403 Forbidden
```

## 5. Common Mistakes

1. Hiding buttons without backend authorization.
2. Relying only on JavaScript role checks.
3. Trusting roles stored in local storage.
4. Protecting frontend routes but not API endpoints.
5. Disabling buttons instead of enforcing permissions.
6. Trusting client-controlled permission values.

Hiding an element alone is not a vulnerability. The security issue occurs when unauthorized backend actions are also permitted.

## 6. Penetration Testing Methodology

1. Identify restricted functionality.
2. Observe administrator requests using Burp Suite.
3. Authenticate as a normal user.
4. Replay the request using the normal user's session.
5. Test direct API access.
6. Test relevant HTTP methods.
7. Compare actual behavior with expected permissions.
8. Verify whether the unauthorized action succeeded.
9. Document evidence and impact.

### Tools

- Burp Suite
- OWASP ZAP
- Browser Developer Tools
- Postman
- cURL

## 7. Prevention

- Enforce authorization on the server.
- Check permissions on every protected request.
- Verify resource ownership.
- Use deny-by-default authorization.
- Never trust client-controlled roles or permissions.
- Apply object-level and function-level authorization.
- Use frontend checks only as an additional user-interface layer.

## 8. AuthZ-Sentinel Connection

AuthZ-Sentinel can:

- Discover frontend restrictions.
- Identify corresponding backend endpoints.
- Test endpoints using different user identities.
- Compare actual responses against a confirmed authorization policy.
- Detect cases where frontend restrictions exist but backend enforcement is missing.

The tool must not assume that observed frontend behavior proves the intended authorization policy.

## 9. Interview Questions

**Q1. Why is client-side access control insufficient?**

Because users can manipulate browser code and send requests directly to backend endpoints.

**Q2. Should client-side checks be removed?**

No. They improve user experience but cannot replace server-side authorization.

**Q3. What is the difference between authentication and authorization?**

Authentication verifies identity; authorization determines permitted actions.

**Q4. What response is commonly returned when an authenticated user lacks permission?**

403 Forbidden.

## Key Takeaway

Client-side access control controls what users see. Server-side access control controls what users are actually allowed to do.

Never rely on the browser as the security boundary.
