# Username Enumeration via Response Timing

## Objective

Identify a valid username by analyzing differences in server response times, then perform a targeted password brute-force attack to gain access to the victim's account.

---

## Lab Overview

This lab demonstrates a timing-based username enumeration vulnerability where the application returns identical authentication error messages but unintentionally leaks account existence through measurable differences in response time.

The lab also introduces IP-based brute-force protection and demonstrates how trusting client-controlled HTTP headers can weaken defensive mechanisms.

---

## Vulnerability

The authentication process performs additional password verification only after determining that a supplied username exists.

As a result, authentication requests for valid usernames require more processing time than requests for invalid usernames, creating an observable timing side channel.

Although the application displays identical error messages, attackers can accurately identify legitimate usernames by measuring response times.

**OWASP Top 10 (2021): A07 - Identification and Authentication Failures**

**CWE-208: Observable Timing Discrepancy**

---

## Exploitation Steps

1. Intercepted a login request using Burp Suite Proxy.
2. Sent the request to Burp Intruder.
3. Added an `X-Forwarded-For` HTTP header to bypass the application's IP-based brute-force protection.
4. Configured a **Pitchfork** attack.
5. Used sequential numbers as payloads for the `X-Forwarded-For` header to simulate unique client IP addresses.
6. Loaded the supplied username list into the username payload position.
7. Replaced the password with a very long string (~100 characters) to maximize processing time for valid usernames.
8. Enabled the **Response received** and **Response completed** timing columns in Burp Intruder.
9. Compared response times for every username and identified the account with consistently higher response latency.
10. Created a second Intruder attack using the discovered username.
11. Continued rotating the `X-Forwarded-For` header to avoid triggering rate limits.
12. Loaded the candidate password list and identified the correct password from the successful **HTTP 302 Redirect** response.
13. Logged into the victim's account and successfully solved the lab.

---

## Root Cause

The application performs different backend operations depending on whether the supplied username exists.

Password verification occurs only after confirming a valid username, resulting in measurable timing differences between valid and invalid authentication attempts.

Additionally, the application incorrectly trusts the `X-Forwarded-For` header when enforcing IP-based brute-force protection, allowing attackers to bypass rate limiting.

---

## Impact

An attacker can:

- Enumerate valid usernames without relying on different error messages.
- Perform targeted password attacks against legitimate accounts.
- Bypass IP-based rate limiting by spoofing trusted HTTP headers.
- Reduce the complexity of authentication attacks.
- Increase the effectiveness of credential stuffing and password spraying attacks.

In real-world environments, timing-based username enumeration is commonly combined with other authentication weaknesses to compromise user accounts.

---

## Mitigation

- Ensure authentication requests execute in constant time regardless of username validity.
- Return identical responses for all authentication failures.
- Never trust client-controlled headers such as `X-Forwarded-For` for security decisions.
- Enforce server-side rate limiting.
- Implement account lockout policies.
- Require Multi-Factor Authentication (MFA).
- Monitor authentication logs for abnormal login patterns.

---

## Tools Used

- Burp Suite Community Edition
- Burp Proxy
- Burp Intruder
- Pitchfork Attack
- HTTP Response Timing Analysis
- Chromium Browser
- PortSwigger Web Security Academy

---

## Skills Practiced

- Authentication Testing
- Username Enumeration
- Timing Side-Channel Analysis
- Burp Intruder
- Pitchfork Attack Configuration
- HTTP Response Analysis
- Rate Limit Assessment
- Web Application Security Assessment

---

## Key Takeaways

This lab demonstrates that authentication systems can unintentionally expose sensitive information through response timing even when visible error messages remain identical.

It also highlights the importance of implementing constant-time authentication logic and avoiding trust in client-controlled HTTP headers for security decisions.

The exercise provided practical experience with Burp Intruder, Pitchfork attacks, timing analysis, response comparison, rate-limit evaluation, and authentication testing techniques commonly used during professional web application security assessments.
