# Object-Level vs Function-Level Authorization

## 1. Object-Level Authorization

Object-level authorization checks whether a user is permitted to access or modify a particular resource.

Examples of objects:

- User profiles
- Bank accounts
- Orders
- Invoices
- Documents
- Medical records
- Projects

### Example

Consider two users:

| User | Owns |
|---|---|
| User A | User ID 101 |
| User B | User ID 102 |

User A sends:

```http
GET /api/users/102
```

The backend must verify whether User A is authorized to access User ID 102.

If User A receives User B's private information without permission, this is an object-level authorization failure.

### Vulnerable implementation

```python
@app.route("/users/<int:user_id>")
def get_user(user_id):
    user = User.query.get(user_id)
    return user.to_dict()
```

The application retrieves the requested user without checking ownership or other access permissions.

### Safer implementation

```python
@app.route("/users/<int:user_id>")
def get_user(user_id):
    user = User.query.get(user_id)

    if user is None:
        return {"error": "Not found"}, 404

    if user.id != current_user.id:
        return {"error": "Forbidden"}, 403

    return user.to_dict()
```

This is a simplified ownership check. Real applications may support sharing, organizational membership, or other authorization policies.

## 2. Function-Level Authorization

Function-level authorization checks whether a user is permitted to execute a particular operation.

Examples:

- Delete a user.
- Create an administrator.
- Export customer records.
- Change account roles.
- Approve a transaction.
- Access administrative functions.

### Example

Suppose only administrators can delete users.

```http
DELETE /api/users/102
```

If a normal user can execute this operation, function-level authorization has failed.

### Vulnerable implementation

```python
@app.route("/users/<int:user_id>", methods=["DELETE"])
def delete_user(user_id):
    User.query.filter_by(id=user_id).delete()
    return {"message": "Deleted"}, 200
```

The backend does not check whether the requester can delete users.

### Safer implementation

```python
@app.route("/users/<int:user_id>", methods=["DELETE"])
def delete_user(user_id):
    if current_user.role != "admin":
        return {"error": "Forbidden"}, 403

    User.query.filter_by(id=user_id).delete()
    return {"message": "Deleted"}, 200
```

A production application should also validate whether the requester is allowed to delete the particular target account.

## 3. Key Differences

| Object-Level | Function-Level |
|---|---|
| Checks resource access | Checks operation permissions |
| Which object can the user access? | Which function can the user execute? |
| Example: View another user's invoice | Example: Delete a user |
| Common failure: BOLA / IDOR | Common failure: BFLA |

**Memory trick:**

- Object = WHAT you are accessing.
- Function = WHAT you are doing.

## 4. Both Checks May Be Required

Consider:

```http
DELETE /api/users/102
```

A secure application may need to verify:

1. Whether the requester can delete users.
2. Whether the requester can delete this particular user.

Having an administrator role does not automatically grant access to every object or tenant.

| Scenario | Function allowed? | Object allowed? | Expected |
|---|---|---|---|
| Admin deletes permitted user | Yes | Yes | Allow |
| Normal user deletes user | No | No | Deny |
| User views own profile | Yes | Yes | Allow |
| User views another private profile | Yes | No | Deny |
| Admin accesses restricted tenant data | Yes | No | Deny |

## 5. Penetration Testing

### Object-Level Testing

1. Create or obtain two authorized test accounts.
2. Identify objects owned by each account.
3. Observe requests containing object identifiers.
4. Change the object identifier using the other test account.
5. Test read, update, and delete operations.
6. Verify whether unauthorized access or modification succeeds.

### Function-Level Testing

1. Identify sensitive application functions.
2. Identify the expected roles for those functions.
3. Authenticate as a lower-privileged user.
4. Send requests to restricted endpoints.
5. Test direct API access.
6. Compare actual behavior with the expected authorization policy.

### Tools

- Burp Suite
- OWASP ZAP
- Postman
- cURL
- Browser Developer Tools

Only test systems you own or have explicit permission to assess.

## 6. Common Developer Mistakes

1. Checking authentication but not object ownership.
2. Checking roles but ignoring resource-level permissions.
3. Hiding administrative URLs instead of protecting endpoints.
4. Trusting object IDs supplied by users.
5. Enforcing permissions only in the frontend.
6. Applying authorization to GET but forgetting DELETE or UPDATE.
7. Checking parent resources but ignoring child resources.

## 7. OWASP API Security Connection

- **BOLA — API1:** Broken Object Level Authorization.
- **BFLA — API5:** Broken Function Level Authorization.

Both are closely related to the broader Broken Access Control category.

## 8. AuthZ-Sentinel Connection

AuthZ-Sentinel can maintain separate testing modules.

### Object-Level Authorization Tester

- Identify object IDs.
- Map objects to test accounts.
- Test cross-user access.
- Test unauthorized read, update, and delete operations.
- Compare results against confirmed ownership policies.

### Function-Level Authorization Tester

- Identify sensitive endpoints.
- Map endpoints to expected roles.
- Test lower-privileged accounts.
- Test anonymous access where applicable.
- Detect unauthorized function execution.

The tool must not infer the intended authorization policy from URL names alone. Expected permissions should be confirmed by a developer or reviewer.

## 9. Interview Questions

**Q1. What is object-level authorization?**

It checks whether a user can access or modify a particular resource.

**Q2. What is function-level authorization?**

It checks whether a user can execute a particular operation.

**Q3. What is the difference between BOLA and BFLA?**

BOLA concerns unauthorized object access; BFLA concerns unauthorized function access.

**Q4. Can an administrator experience an object-level authorization failure?**

Yes. Administrators may still be restricted by tenant, department, or resource-specific policies.

**Q5. Why should authorization be checked on every request?**

Because users can directly access endpoints and bypass frontend restrictions.

## Key Takeaway

Object-level authorization determines which resources a user can access.

Function-level authorization determines which operations a user can perform.

Secure applications enforce both wherever required.
