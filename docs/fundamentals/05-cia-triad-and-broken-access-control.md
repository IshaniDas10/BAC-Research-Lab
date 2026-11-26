# 05 — CIA Triad and Broken Access Control

## 1. Introduction

The CIA Triad is a fundamental information security model consisting of three principles:

- Confidentiality
- Integrity
- Availability

It defines the three major security objectives for protecting information and systems.

## 2. Confidentiality

Confidentiality ensures that information is accessible only to authorized users and systems.

Example:
- User A can access their own bank account.
- User B can access their own bank account.
- User A cannot access User B's private account information.

**BAC connection:** Unauthorized access to another user's private data violates confidentiality.

Related vulnerability: IDOR/BOLA.

## 3. Integrity

Integrity ensures that information remains accurate, consistent and protected against unauthorized modification.

Example:
- A customer can view their order.
- A customer cannot change the order price.
- Only authorized employees can approve refunds.

**BAC connection:** Unauthorized modification of another user's records violates integrity.

Related vulnerabilities:
- Mass assignment.
- Privilege escalation.
- Unauthorized resource modification.

## 4. Availability

Availability ensures that authorized users can access systems and information when required.

Example:
- An administrator manages application resources.
- Unauthorized users must not delete or disable those resources.

**BAC connection:** Unauthorized deletion or modification can affect resource availability.

Note: DDoS is an availability attack but is not automatically a Broken Access Control vulnerability.

## 5. CIA Triad Comparison

| Principle | Security objective | BAC example |
|---|---|---|
| Confidentiality | Prevent unauthorized disclosure | Reading another user's data |
| Integrity | Prevent unauthorized modification | Changing another user's records |
| Availability | Maintain authorized access | Deleting or disabling resources |

## 6. Relationship Between BAC and CIA

Broken Access Control can affect all three CIA principles.

A single authorization failure may allow an attacker to:
- Read sensitive information.
- Modify protected records.
- Delete important resources.

The actual impact depends on the permissions incorrectly granted.

## 7. Pentesting Perspective

During BAC testing, evaluate:

1. Unauthorized read operations.
2. Unauthorized create operations.
3. Unauthorized update operations.
4. Unauthorized delete operations.
5. Unauthorized administrative actions.

Use authorized accounts and controlled test data.

## 8. Connection to AuthZ-Sentinel

AuthZ-Sentinel can categorize findings according to their potential CIA impact.

Examples:
- Unauthorized data disclosure → Confidentiality.
- Unauthorized record modification → Integrity.
- Unauthorized resource deletion → Availability.

The impact must be confirmed using actual evidence rather than inferred only from an HTTP method or response code.

## 9. Key Takeaway

**Confidentiality protects information from unauthorized access.**

**Integrity protects information from unauthorized modification.**

**Availability ensures legitimate users can access resources when needed.**

Broken Access Control can compromise any or all of these principles.

---

**Status:** Completed — Fundamentals, Topic 05  
**Category:** Information Security Fundamentals
