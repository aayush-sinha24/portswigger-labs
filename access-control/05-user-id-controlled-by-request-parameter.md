# User ID Controlled by Request Parameter

## Objective

Identify and exploit an Insecure Direct Object Reference (IDOR) vulnerability by manipulating a user identifier passed within a request parameter.

---

## Lab Overview

This lab demonstrates an IDOR vulnerability where the application exposes internal object identifiers through client-controlled request parameters.

Instead of validating ownership of the requested resource, the application directly retrieves user-specific information based solely on the supplied identifier. By modifying this identifier, an attacker can access sensitive information belonging to other users.

---

## Vulnerability

The application uses a client-controlled user identifier to retrieve sensitive account information without verifying whether the authenticated user is authorized to access the requested resource.

As a result, changing the identifier allows unauthorized access to another user's data.

**OWASP Top 10:** A01:2021 – Broken Access Control

**CWE:** CWE-639 – Authorization Bypass Through User-Controlled Key

---

## Exploitation Steps

1. Logged in using a standard user account.
2. Navigated to the account information page.
3. Identified the user identifier passed within the request URL.
4. Modified the identifier to reference another user's account.
5. Forwarded the modified request.
6. Retrieved sensitive information belonging to another user.
7. Used the exposed information to successfully complete the lab.

---

## Root Cause

The application directly trusts client-supplied object identifiers without validating resource ownership.

Although authentication is enforced, authorization checks are missing, allowing authenticated users to access resources belonging to other users.

---

## Impact

Successful exploitation allows an attacker to:

* Access confidential user information.
* Retrieve sensitive account data.
* Violate user privacy.
* Perform unauthorized actions against other accounts.
* Escalate attacks using exposed credentials or API keys.

---

## Mitigation

* Enforce server-side authorization for every object request.
* Validate resource ownership before returning sensitive information.
* Avoid exposing sequential identifiers where possible.
* Implement indirect object references or random identifiers.
* Follow the principle of least privilege.

---

## Tools Used

* Burp Suite Community Edition
* Chromium Browser
* PortSwigger Web Security Academy

---

## Skills Practiced

* IDOR Discovery
* Access Control Testing
* Authorization Assessment
* Parameter Manipulation
* Burp Suite Repeater

---

## Key Takeaways

Authentication alone is not sufficient to protect sensitive resources. Every request referencing user-controlled object identifiers must be validated on the server to ensure that users can only access resources they are explicitly authorized to view.
