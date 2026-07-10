# Password Reset Broken Logic

## Objective

Exploit a flaw in the password reset workflow to change another user's password and gain unauthorized access to their account.

---

## Lab Overview

This lab demonstrates a business logic vulnerability in the password reset functionality. The application incorrectly trusts user-controlled input during the reset process, allowing an attacker to manipulate the workflow and reset another user's password without proper authorization.

Unlike traditional authentication vulnerabilities, this issue arises from flawed application logic rather than weak cryptography or insufficient access controls.

---

## Vulnerability

The password reset mechanism fails to properly bind the reset request to the authenticated user or the intended account.

By modifying request parameters during the password reset process, an attacker can reset the password of another user without possessing their original credentials.

**OWASP Top 10 (2021): A01 - Broken Access Control**

**OWASP Top 10 (2021): A04 - Insecure Design**

**CWE-640: Weak Password Recovery Mechanism for Forgotten Password**

---

## Exploitation Steps

1. Intercepted the password reset request using Burp Suite.
2. Examined the request parameters used during the password reset workflow.
3. Identified that the username parameter could be modified before the request reached the server.
4. Replaced the original username with the victim's username.
5. Forwarded the modified request.
6. Successfully reset the victim's password without possessing their previous password.
7. Logged into the victim's account using the newly assigned password.
8. Accessed the account page and successfully solved the lab.

---

## Root Cause

The application trusts client-controlled parameters during the password reset process instead of securely associating the reset operation with a verified user identity.

Critical authorization checks are missing, allowing attackers to modify account identifiers before the request is processed.

Business logic flaws like this cannot be prevented by encryption alone—they require secure workflow validation.

---

## Impact

An attacker can:

- Reset passwords for arbitrary users.
- Gain unauthorized account access.
- Completely compromise user accounts.
- Escalate privileges if administrative accounts are targeted.
- Bypass the intended password recovery process.

In production environments, this vulnerability could lead to full account takeover without requiring password guessing or credential theft.

---

## Mitigation

- Never trust client-controlled identifiers during password reset workflows.
- Associate password reset requests with cryptographically secure server-side tokens.
- Verify ownership before allowing password changes.
- Perform authorization checks at every stage of the reset process.
- Expire reset tokens after a single successful use.
- Log and monitor password reset events for suspicious activity.

---

## Tools Used

- Burp Suite Community Edition
- Burp Proxy
- Burp Repeater
- Chromium Browser
- PortSwigger Web Security Academy

---

## Skills Practiced

- Business Logic Testing
- Password Reset Security Assessment
- Request Manipulation
- Authentication Testing
- HTTP Request Analysis
- Burp Proxy
- Burp Repeater
- Account Takeover Assessment

---

## Key Takeaways

This lab highlights that secure authentication depends not only on strong passwords and encryption but also on correctly designed application workflows.

Even when cryptographic mechanisms are secure, weak business logic can allow attackers to bypass intended security controls and fully compromise user accounts.

The exercise reinforced the importance of validating every step in sensitive workflows and demonstrated how business logic vulnerabilities are frequently encountered during professional web application penetration tests.
