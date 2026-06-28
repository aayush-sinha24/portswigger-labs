# User Role Can Be Modified in User Profile

## Objective

Identify and exploit an authorization vulnerability where user role information can be modified through profile management functionality.

---

## Lab Overview

This lab demonstrates a critical Broken Access Control vulnerability in which sensitive authorization attributes are exposed to the client during profile updates.

Instead of restricting modifications to privileged fields, the application accepts user-supplied role values without validating whether the authenticated user is authorized to change them. By manipulating the request, an attacker can assign administrative privileges to their own account.

---

## Vulnerability

The application exposes privileged attributes within a user profile update request and fails to enforce server-side authorization before applying changes.

Because the server trusts client-controlled input, unauthorized users can modify sensitive role information and escalate their privileges.

**OWASP Top 10:** A01:2021 – Broken Access Control

**CWE:** CWE-639 – Authorization Bypass Through User-Controlled Key

---

## Exploitation Steps

1. Logged in using a standard user account.
2. Navigated to the profile management functionality.
3. Intercepted the profile update request using Burp Suite.
4. Identified the parameter responsible for storing the user's role.
5. Modified the role value to administrator.
6. Forwarded the manipulated request.
7. Successfully gained administrative privileges and completed the lab.

---

## Root Cause

The application permits users to modify security-sensitive attributes without verifying whether they possess sufficient privileges.

Authorization decisions should always be enforced on the server, and sensitive fields must never be trusted when supplied by the client.

---

## Impact

An attacker can:

* Escalate privileges to administrator.
* Bypass authorization mechanisms.
* Gain unrestricted access to administrative functionality.
* Modify privileged application settings.
* Compromise the confidentiality and integrity of application data.

---

## Mitigation

* Never expose privileged attributes within user-editable forms.
* Ignore client-supplied authorization fields during profile updates.
* Validate authorization on every sensitive operation.
* Implement centralized Role-Based Access Control (RBAC).
* Apply the principle of least privilege throughout the application.

---

## Tools Used

* Burp Suite Community Edition
* Chromium Browser
* PortSwigger Web Security Academy

---

## Skills Practiced

* Access Control Testing
* Privilege Escalation
* HTTP Request Manipulation
* Authorization Assessment
* Burp Suite Repeater

---

## Key Takeaways

Sensitive authorization attributes should never be modifiable by end users. Applications must enforce authorization on the server side and ignore any client-controlled role information during profile updates to prevent privilege escalation attacks.
