# Password Reset Poisoning via Middleware

## Objective

Exploit improper trust in user-controlled HTTP headers during the password reset process to hijack a password reset link and gain unauthorized access to another user's account.

---

## Lab Overview

This lab demonstrates a Host Header Injection vulnerability in the password reset functionality. The application trusts user-controlled HTTP headers when generating password reset links.

By manipulating the request before it reaches the application, an attacker can cause the password reset email to contain a malicious domain under the attacker's control. When the victim clicks the poisoned reset link, the reset token is leaked to the attacker, allowing complete account takeover.

---

## Vulnerability

The application generates password reset URLs using untrusted client-supplied headers instead of trusted server-side configuration.

An attacker can manipulate these headers to poison the password reset link and capture the victim's reset token.

OWASP Top 10 (2021): A01 - Broken Access Control

OWASP Top 10 (2021): A05 - Security Misconfiguration

CWE-346: Origin Validation Error

---

## Exploitation Steps

1. Intercepted the password reset request using Burp Suite.
2. Identified that the application trusted a user-controlled HTTP header while generating the reset link.
3. Modified the header to point to the attacker-controlled exploit server.
4. Forwarded the modified request.
5. Waited for the victim to access the poisoned password reset link.
6. Retrieved the leaked reset token from the exploit server access logs.
7. Used the captured token to reset the victim's password.
8. Logged into the victim's account and successfully solved the lab.

---

## Root Cause

The application constructs password reset URLs using client-controlled request headers instead of trusted server-side values.

Security-sensitive URLs should always be generated from trusted application configuration. Accepting user-controlled host information allows attackers to redirect password reset tokens to malicious domains.

---

## Impact

An attacker can:

- Hijack password reset emails.
- Capture password reset tokens.
- Completely take over user accounts.
- Bypass normal authentication controls.
- Compromise privileged accounts if targeted.

In production environments, this vulnerability can lead to full account compromise with minimal user interaction.

---

## Mitigation

- Generate password reset links using trusted server-side configuration.
- Never trust client-controlled Host or forwarding headers for security decisions.
- Validate and whitelist allowed hostnames.
- Use short-lived, single-use password reset tokens.
- Notify users of password reset requests and completed password changes.
- Monitor for abnormal password reset activity.

---

## Tools Used

- Burp Suite Community Edition
- Burp Proxy
- Burp Repeater
- Chromium Browser
- Exploit Server
- PortSwigger Web Security Academy

---

## Skills Practiced

- Authentication Testing
- Password Reset Security Assessment
- Host Header Injection
- HTTP Header Manipulation
- Burp Repeater
- Token Security Analysis
- Account Takeover Assessment

---

## Key Takeaways

This lab demonstrates that password reset functionality is a high-value target and must never rely on client-controlled request headers when generating security-sensitive links.

It also reinforces that small implementation mistakes in password recovery workflows can result in complete account compromise, even when the authentication mechanism itself is otherwise secure.
