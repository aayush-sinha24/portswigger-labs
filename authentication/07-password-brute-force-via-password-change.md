# Password Brute-force via Password Change

## Objective

Exploit flawed password change logic to brute-force another user's current password and gain unauthorized access to their account.

---

## Lab Overview

This lab demonstrates a business logic vulnerability in the password change functionality. The application validates the current password before updating it but returns distinguishable responses depending on whether the supplied current password is correct.

An attacker who has access to a valid session can repeatedly submit password change requests with different current passwords. Once the correct password is identified, the attacker changes the victim's password and gains full control of the account.

---

## Vulnerability

The password change endpoint leaks information by producing different responses for valid and invalid current passwords.

This response discrepancy enables attackers to brute-force the victim's existing password without triggering effective protections.

OWASP Top 10 (2021): A01 - Broken Access Control

OWASP Top 10 (2021): A07 - Identification and Authentication Failures

CWE-307: Improper Restriction of Excessive Authentication Attempts

---

## Exploitation Steps

1. Logged into the provided user account.
2. Intercepted the password change request using Burp Suite.
3. Sent the request to Burp Repeater for analysis.
4. Identified that the application returned different responses depending on whether the supplied current password was correct.
5. Sent the request to Burp Intruder.
6. Configured the current password parameter as the attack position.
7. Used the provided password wordlist to brute-force the current password.
8. Identified the valid password from the unique server response.
9. Changed the victim's password using the discovered current password.
10. Logged into the victim's account with the new credentials.
11. Successfully solved the lab.

---

## Root Cause

The application exposes authentication state through inconsistent password change responses.

Instead of returning identical responses for every failed password change attempt, the application reveals when the supplied current password is correct. This information disclosure enables efficient brute-force attacks.

Additionally, the endpoint lacks sufficient rate limiting and account protection mechanisms.

---

## Impact

An attacker can:

- Brute-force user passwords.
- Reset another user's password.
- Take over legitimate user accounts.
- Escalate privileges if privileged accounts are targeted.
- Bypass intended authentication protections.

In production environments, this vulnerability can lead to complete account compromise despite strong password policies.

---

## Mitigation

- Return identical responses for all failed password change attempts.
- Implement rate limiting on password change endpoints.
- Require Multi-Factor Authentication (MFA) for sensitive account operations.
- Lock or delay repeated failed password verification attempts.
- Log and monitor abnormal password change activity.
- Notify users whenever their password is successfully changed.

---

## Tools Used

- Burp Suite Community Edition
- Burp Proxy
- Burp Repeater
- Burp Intruder
- Chromium Browser
- PortSwigger Web Security Academy

---

## Skills Practiced

- Authentication Testing
- Business Logic Testing
- Password Change Security Assessment
- Burp Intruder
- HTTP Request Analysis
- Response Comparison
- Account Takeover Assessment

---

## Key Takeaways

This lab demonstrates that password change functionality is part of the application's authentication surface and must be protected as carefully as the login endpoint.

It also highlights how small differences in server responses can unintentionally disclose sensitive information, enabling attackers to brute-force credentials and compromise user accounts.
