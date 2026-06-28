# Insecure Direct Object References (IDOR)

## Objective

Identify and exploit an Insecure Direct Object Reference (IDOR) vulnerability that allows unauthorized access to another user's sensitive information by manipulating a direct object reference.

---

## Lab Overview

This lab demonstrates a classic IDOR vulnerability where the application exposes direct object references without validating whether the authenticated user is authorized to access the requested resource.

By modifying a user-controlled identifier, sensitive information belonging to another user can be retrieved, resulting in unauthorized data disclosure.

---

## Vulnerability

The application directly references internal objects using user-controlled identifiers while failing to verify resource ownership on the server.

Although authentication is enforced, authorization checks are missing, allowing authenticated users to access objects belonging to other users.

**OWASP Top 10:** A01:2021 – Broken Access Control

**CWE:** CWE-639 – Authorization Bypass Through User-Controlled Key

---

## Exploitation Steps

1. Logged in using a standard user account.
2. Explored the application to identify requests containing object references.
3. Located a user-controlled identifier within the request.
4. Modified the identifier to reference another user's resource.
5. Sent the modified request using Burp Suite.
6. Retrieved sensitive information belonging to another user.
7. Successfully completed the lab by exploiting the authorization weakness.

---

## Root Cause

The application performs authentication but fails to verify whether the authenticated user is authorized to access the requested resource.

The server trusts user-controlled object identifiers without validating ownership, resulting in unauthorized data exposure.

---

## Impact

Successful exploitation may allow an attacker to:

* Access confidential user information.
* Retrieve API keys, personal information or private documents.
* Enumerate other user accounts.
* Perform unauthorized actions against other users.
* Escalate attacks using exposed sensitive data.

IDOR vulnerabilities are frequently identified during professional penetration tests because they often expose high-value business data.

---

## Mitigation

* Validate resource ownership on every request.
* Enforce server-side authorization checks.
* Never rely solely on user-controlled identifiers.
* Use indirect object references where appropriate.
* Perform access control validation before returning sensitive resources.

---

## Tools Used

* Burp Suite Community Edition
* Chromium Browser
* PortSwigger Web Security Academy

---

## Skills Practiced

* IDOR Discovery
* Authorization Testing
* Object Reference Manipulation
* Access Control Assessment
* Burp Suite Repeater

---

## Key Takeaways

IDOR vulnerabilities occur when applications expose direct references to internal objects without validating whether the authenticated user is authorized to access them. Every request referencing user-controlled identifiers must be protected by robust server-side authorization checks.
