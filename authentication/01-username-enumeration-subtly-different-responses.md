# Username Enumeration via Subtly Different Responses

## Objective

Identify a valid username by analyzing subtle differences in authentication responses, then perform a targeted password brute-force attack to gain access to the victim's account.

---

## Lab Overview

This lab demonstrates a username enumeration vulnerability where the application returns slightly different responses when a valid username is supplied. Although the differences are minimal, they allow an attacker to distinguish existing accounts before attempting password brute-force attacks.

Instead of returning identical responses for both valid and invalid usernames, the application unintentionally leaks information through subtle variations in its HTTP responses.

---

## Vulnerability

The authentication mechanism exposes account existence by generating distinguishable responses for valid usernames.

An attacker can automate login attempts, compare server responses, and accurately identify legitimate usernames before initiating password attacks.

**OWASP Top 10 (2021):** A07 – Identification and Authentication Failures

**CWE-203:** Observable Discrepancy

---

## Exploitation Steps

1. Intercepted an invalid login request using Burp Suite.
2. Sent the request to Burp Intruder.
3. Replaced the username parameter with the provided candidate username list.
4. Configured **Grep - Extract** to capture the authentication error message.
5. Started the Intruder attack and compared extracted responses.
6. Identified the valid username through a subtle difference in the returned error message.
7. Performed a second Intruder attack using the identified username and the provided password list.
8. Detected the correct password from the successful authentication response (HTTP 302 redirect).
9. Logged into the application and successfully solved the lab.

---

## Root Cause

The application generates different server responses depending on whether the supplied username exists.

Although the variation is extremely small, automated response comparison tools such as Burp Intruder make the discrepancy easy to identify.

Authentication systems should never reveal whether a username exists before successful authentication.

---

## Impact

An attacker can:

- Enumerate valid usernames.
- Significantly reduce brute-force attack complexity.
- Launch targeted password attacks against legitimate accounts.
- Increase the likelihood of successful credential attacks.

In real-world environments, this vulnerability is commonly chained with password spraying, credential stuffing, and brute-force attacks.

---

## Mitigation

- Return identical responses for both valid and invalid usernames.
- Ensure response length, formatting, and timing remain consistent.
- Implement rate limiting and account lockout mechanisms.
- Enable Multi-Factor Authentication (MFA).
- Monitor and detect repeated authentication attempts.

---

## Tools Used

- Burp Suite Community Edition
- Burp Intruder
- Grep - Extract
- Chromium Browser
- PortSwigger Web Security Academy

---

## Skills Practiced

- Authentication Testing
- Username Enumeration
- HTTP Response Analysis
- Burp Suite Intruder
- Grep - Extract Configuration
- Password Brute Force Methodology
- Web Application Security Assessment

---

## Key Takeaways

This lab demonstrates that even minor inconsistencies in authentication responses can expose valid usernames. Small implementation mistakes can significantly weaken authentication security by enabling targeted brute-force attacks.

The exercise also reinforced practical experience with Burp Suite Intruder, automated response comparison, and systematic authentication testing techniques commonly used during professional web application security assessments.
