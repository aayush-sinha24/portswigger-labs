# User Role Controlled by Request Parameter

## Objective

Identify and exploit a privilege escalation vulnerability where the application determines user roles using a client-controlled request parameter.

---

## Lab Overview

This lab demonstrates a vertical privilege escalation vulnerability caused by trusting user-supplied request parameters for authorization decisions.

Instead of validating user roles on the server, the application relies on a client-controlled parameter to determine whether a user possesses administrative privileges. By modifying this parameter, an attacker can gain unauthorized access to administrator functionality.

---

## Vulnerability

The application stores the user's authorization level within a request parameter that can be modified by the client.

Since the server trusts this value without performing independent authorization checks, an attacker can manipulate the parameter to obtain elevated privileges.

**OWASP Top 10:** A01:2021 – Broken Access Control

**CWE:** CWE-639 – Authorization Bypass Through User-Controlled Key

---

## Exploitation Steps

1. Authenticated as a low-privileged user.
2. Intercepted the request using Burp Suite.
3. Identified the request parameter responsible for determining the user's role.
4. Modified the role parameter from a standard user value to an administrator value.
5. Forwarded the modified request to the application.
6. Successfully gained access to administrative functionality.
7. Executed the required administrative action to complete the lab.

---

## Root Cause

The application incorrectly trusts user-controlled input when making authorization decisions.

Authorization must always be enforced using server-side session data or trusted backend mechanisms. Client-controlled parameters should never determine user privileges.

---

## Impact

Successful exploitation allows an attacker to:

* Escalate privileges to administrator.
* Access restricted administrative interfaces.
* Perform unauthorized privileged operations.
* Modify or delete sensitive application data.
* Completely bypass intended authorization controls.

In a production environment, this vulnerability could lead to full application compromise.

---

## Mitigation

* Store user roles exclusively on the server.
* Validate authorization for every privileged request.
* Ignore client-controlled role or privilege parameters.
* Implement centralized Role-Based Access Control (RBAC).
* Follow the principle of least privilege.

---

## Tools Used

* Burp Suite Community Edition
* Chromium Browser
* PortSwigger Web Security Academy

---

## Skills Practiced

* Access Control Testing
* Vertical Privilege Escalation
* Request Parameter Manipulation
* Authorization Testing
* Burp Suite Repeater

---

## Key Takeaways

Authorization decisions must never rely on client-controlled request parameters. Any privilege information supplied by the client can be modified by an attacker, making server-side authorization validation essential for protecting administrative functionality.
