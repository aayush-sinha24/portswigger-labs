# Unprotected Admin Functionality

## Objective

Identify and exploit an administrative endpoint that lacks authentication and authorization controls, allowing unauthorized users to access sensitive functionality.

---

## Lab Overview

This lab demonstrates a Broken Access Control vulnerability where the administrative interface is directly accessible without verifying whether the requesting user has administrator privileges.

By discovering the exposed administrative endpoint, an attacker can perform privileged operations that should only be available to administrators.

---

## Vulnerability

The application exposes administrative functionality without enforcing server-side authorization checks.

Since the endpoint is publicly accessible, any user who discovers the URL can execute administrative actions without authentication.

**OWASP Top 10:** A01:2021 – Broken Access Control

**CWE:** CWE-862 – Missing Authorization

---

## Exploitation Steps

1. Explored the application and identified publicly accessible resources.
2. Discovered the exposed administrator endpoint.
3. Navigated directly to the administrative interface.
4. Accessed privileged functionality without authentication.
5. Performed the required administrative action to successfully complete the lab.

---

## Root Cause

The application assumes that the administrative endpoint is difficult to discover and therefore does not perform authorization checks on incoming requests.

Security must always be enforced on the server side rather than relying on hidden URLs.

---

## Impact

An attacker can:

* Access administrative functionality.
* Perform privileged operations.
* Modify sensitive application data.
* Compromise the integrity of the application.

In a real-world environment, this vulnerability could result in complete administrative compromise.

---

## Mitigation

* Enforce server-side authorization for every administrative request.
* Deny access to users without administrative privileges.
* Follow the principle of least privilege.
* Avoid relying on obscurity to protect sensitive endpoints.
* Regularly audit exposed administrative interfaces.

---

## Tools Used

* Burp Suite Community Edition
* Chromium Browser
* PortSwigger Web Security Academy

---

## Skills Practiced

* Access Control Testing
* Forced Browsing
* Administrative Endpoint Discovery
* Authorization Testing
* Web Application Security Assessment

---

## Key Takeaways

This lab highlights the importance of implementing proper authorization checks on every privileged endpoint. Hidden URLs should never be treated as a security mechanism, as attackers can easily discover exposed administrative functionality through enumeration or predictable paths.
