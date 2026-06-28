# Unprotected Admin Functionality with Unpredictable URL

## Objective

Identify a hidden administrative interface and exploit the lack of authorization controls to access privileged functionality.

---

## Lab Overview

This lab demonstrates that hiding an administrative endpoint behind an unpredictable URL does not provide security. Although the endpoint is not directly linked within the application, it can still be discovered through publicly accessible resources.

Once the hidden administrative interface is identified, administrative actions can be performed because the application fails to enforce proper server-side authorization.

---

## Vulnerability

The application relies on an unguessable URL to protect sensitive administrative functionality instead of implementing proper authorization checks.

Attackers can discover hidden endpoints by inspecting client-side resources such as JavaScript files, HTML source code, or other publicly accessible assets.

**OWASP Top 10:** A01:2021 – Broken Access Control

**CWE:** CWE-862 – Missing Authorization

---

## Exploitation Steps

1. Explored publicly accessible application resources.
2. Inspected client-side files to identify references to hidden administrative endpoints.
3. Located the administrative interface through an exposed URL.
4. Navigated directly to the hidden endpoint.
5. Accessed privileged functionality without authorization.
6. Executed the required administrative action to successfully complete the lab.

---

## Root Cause

The application assumes that keeping administrative endpoints hidden is sufficient to prevent unauthorized access.

Security through obscurity is not an effective defense. Every privileged endpoint must independently verify the user's authorization before processing requests.

---

## Impact

An attacker who discovers the hidden endpoint can:

* Access restricted administrative functionality.
* Perform unauthorized privileged operations.
* Modify or delete sensitive application data.
* Fully compromise administrative features.

---

## Mitigation

* Enforce server-side authorization on every administrative endpoint.
* Do not rely on hidden or unpredictable URLs for security.
* Remove unnecessary endpoint references from client-side code.
* Regularly review publicly accessible resources for sensitive information disclosure.
* Follow the principle of least privilege.

---

## Tools Used

* Burp Suite Community Edition
* Chromium Browser
* PortSwigger Web Security Academy

---

## Skills Practiced

* Hidden Endpoint Discovery
* JavaScript Analysis
* Access Control Testing
* Forced Browsing
* Authorization Assessment

---

## Key Takeaways

Administrative functionality should never depend on secret URLs for protection. Even if an endpoint is difficult to discover, attackers can identify hidden resources through client-side analysis, directory enumeration, or information leakage. Proper server-side authorization remains the only reliable defense against unauthorized administrative access.
