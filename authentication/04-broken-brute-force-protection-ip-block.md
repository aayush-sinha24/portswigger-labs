# Broken Brute-Force Protection (IP Block)

## Objective

Exploit a logic flaw in the application's brute-force protection mechanism to bypass account lockout and gain unauthorized access to the victim's account.

---

## Lab Overview

This lab demonstrates a flawed brute-force protection mechanism that temporarily blocks repeated failed login attempts. Although the application enforces an IP-based timeout after multiple incorrect passwords, the protection can be bypassed by abusing the application's authentication workflow.

Instead of continuously sending failed login attempts, the attacker periodically performs a successful login using known valid credentials. This resets the failed-attempt counter, allowing the brute-force attack to continue indefinitely.

---

## Vulnerability

The application incorrectly assumes that every successful login represents legitimate user activity and resets the brute-force protection state without validating the overall authentication pattern.

By alternating failed login attempts against the victim with successful authentication to another account, an attacker can bypass the intended rate-limiting and account lockout mechanism.

**OWASP Top 10 (2021):** A07 - Identification and Authentication Failures

**CWE-307:** Improper Restriction of Excessive Authentication Attempts

---

## Exploitation Steps

1. Intercepted the login request using Burp Suite.
2. Identified that the application temporarily blocked repeated failed login attempts.
3. Observed that a successful login reset the brute-force protection counter.
4. Used the provided valid credentials to periodically reset the failed-attempt counter.
5. Automated the request sequence by alternating failed login attempts for the victim account with successful authentication requests.
6. Generated the required username/password sequence to avoid triggering permanent lockout.
7. Sent the generated requests using Burp Intruder.
8. Monitored the responses and identified the successful authentication (HTTP 302 Redirect).
9. Logged into the victim's account and successfully solved the lab.

---

## Root Cause

The brute-force protection mechanism trusts successful authentication events without considering the broader attack pattern.

Rather than tracking suspicious authentication behavior independently for each targeted account, the application resets its protection logic whenever any successful login occurs. This enables attackers to continuously restart the brute-force process and defeat the intended defense.

---

## Impact

An attacker can:

- Bypass brute-force protection.
- Perform unlimited password guessing.
- Defeat account lockout mechanisms.
- Increase the likelihood of credential compromise.
- Gain unauthorized access to user accounts.

In real-world environments, weaknesses like this frequently enable credential stuffing and password spraying attacks against internet-facing applications.

---

## Mitigation

- Track failed login attempts independently for each protected account.
- Avoid resetting brute-force counters based solely on successful authentication.
- Implement adaptive rate limiting and account lockout mechanisms.
- Apply IP reputation and behavioral analysis.
- Require Multi-Factor Authentication (MFA) for sensitive accounts.
- Monitor authentication events for abnormal login patterns.

---

## Tools Used

- Burp Suite Community Edition
- Burp Proxy
- Burp Intruder
- Python (request sequence generation)
- Chromium Browser
- PortSwigger Web Security Academy

---

## Skills Practiced

- Authentication Testing
- Brute-Force Assessment
- Login Protection Analysis
- Burp Intruder
- Python Automation
- HTTP Request Analysis
- Business Logic Testing
- Web Application Security Assessment

---

## Key Takeaways

This lab demonstrates that authentication security depends not only on limiting failed login attempts but also on correctly implementing defensive logic.

Even well-intentioned security controls can become ineffective when state management is flawed. Understanding how authentication workflows behave is often more valuable than simply increasing brute-force limits, as attackers frequently exploit weaknesses in application logic rather than cryptographic protections.
