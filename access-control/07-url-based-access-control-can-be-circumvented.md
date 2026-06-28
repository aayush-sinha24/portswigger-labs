# URL-based Access Control Can Be Circumvented

## Objective

Identify and exploit an authorization bypass vulnerability where access control is enforced only through URL restrictions instead of proper server-side authorization.

---

## Lab Overview

This lab demonstrates a Broken Access Control vulnerability in which sensitive administrative functionality is protected only by restricting access to specific URLs.

By manipulating request headers and directly accessing administrative endpoints, it is possible to bypass the application's URL-based access restrictions and execute privileged operations without administrative privileges.

---

## Vulnerability

The application relies on URL-based restrictions to protect privileged functionality instead of validating user permissions on the server.

Since authorization decisions are not consistently enforced, an attacker can manipulate specially handled request headers to bypass the URL restrictions and access administrative resources.

**OWASP Top 10:** A01:2021 – Broken Access Control

**CWE:** CWE-285 – Improper Authorization

---

## Exploitation Steps

1. Logged in using a standard user account.
2. Identified an administrative endpoint protected through URL-based restrictions.
3. Intercepted the request using Burp Suite.
4. Modified the request by supplying the appropriate URL rewrite header.
5. Forwarded the manipulated request to the server.
6. Successfully bypassed the URL-based access restriction.
7. Performed the required administrative action to complete the lab.

---

## Root Cause

The application incorrectly assumes that restricting access to certain URLs is sufficient to protect administrative functionality.

Authorization decisions must always be enforced by the application logic rather than relying on request routing, reverse proxies, or URL filtering mechanisms.

---

## Impact

An attacker can:

* Bypass administrative URL restrictions.
* Access privileged functionality.
* Perform unauthorized administrative actions.
* Compromise sensitive application resources.
* Escalate privileges without possessing administrator credentials.

---

## Mitigation

* Perform authorization checks on every privileged request.
* Do not rely solely on URL filtering or reverse proxy rules.
* Ignore client-controlled routing headers unless explicitly required.
* Validate user permissions before processing sensitive operations.
* Apply centralized server-side authorization controls.

---

## Tools Used

* Burp Suite Community Edition
* Chromium Browser
* PortSwigger Web Security Academy

---

## Skills Practiced

* Access Control Testing
* Authorization Bypass
* HTTP Header Manipulation
* Administrative Endpoint Discovery
* Burp Suite Repeater

---

## Key Takeaways

Restricting access to administrative functionality based only on URL paths is not a reliable security control. Every privileged request must undergo independent server-side authorization to ensure that only authorized users can access sensitive functionality.
