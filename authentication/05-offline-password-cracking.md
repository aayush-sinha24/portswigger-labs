# Offline Password Cracking

## Objective

Steal another user's stay-logged-in cookie using Cross-Site Scripting (XSS), recover the hashed credentials, crack the password offline, and gain unauthorized access to the victim's account.

---

## Lab Overview

This lab demonstrates how weak implementation of persistent login cookies can allow attackers to steal authentication data and recover user passwords offline.

An attacker first injects a malicious JavaScript payload to steal the victim's stay-logged-in cookie. Because the cookie contains predictable authentication information, it can be decoded and the password hash extracted. The attacker then cracks the hash using a wordlist and logs in as the victim.

---

## Vulnerability

The application stores predictable authentication data inside the stay-logged-in cookie.

After stealing the cookie through Cross-Site Scripting (XSS), an attacker can recover the password hash and perform offline password cracking without interacting with the target application.

OWASP Top 10 (2021): A07 - Identification and Authentication Failures

OWASP Top 10 (2021): A03 - Injection

CWE-522: Insufficiently Protected Credentials

---

## Exploitation Steps

1. Identified that the application uses a stay-logged-in cookie.
2. Injected a JavaScript payload through the comment functionality.
3. Exfiltrated the victim's cookie to the exploit server.
4. Retrieved the stolen cookie from the exploit server access logs.
5. Decoded the cookie and extracted the username and password hash.
6. Cracked the hash offline using a password cracking tool and the provided wordlist.
7. Logged into the victim's account using the recovered credentials.
8. Successfully solved the lab.

---

## Root Cause

Sensitive authentication information is stored inside client-side cookies without sufficient protection.

Even though the password itself is not stored directly, exposing a reusable password hash allows attackers to perform offline cracking attacks without triggering server-side detection or rate limiting.

Authentication cookies should never contain recoverable credential material.

---

## Impact

An attacker can:

- Steal authentication cookies using XSS.
- Recover password hashes from stolen cookies.
- Perform offline password cracking.
- Completely compromise user accounts.
- Bypass online brute-force protections.

In real-world environments, combining XSS with weak authentication cookies can result in full account compromise.

---

## Mitigation

- Never store password hashes inside client-side cookies.
- Use random session identifiers instead of predictable authentication data.
- Protect against Cross-Site Scripting (XSS).
- Mark cookies as HttpOnly, Secure, and SameSite where appropriate.
- Use strong password hashing algorithms such as Argon2, bcrypt, or scrypt.
- Implement Multi-Factor Authentication (MFA).

---

## Tools Used

- Burp Suite Community Edition
- Burp Proxy
- Chromium Browser
- Exploit Server
- Hash Cracking Tool
- PortSwigger Web Security Academy

---

## Skills Practiced

- Authentication Testing
- Cross-Site Scripting (XSS)
- Cookie Analysis
- Session Security Assessment
- Offline Password Cracking
- Hash Analysis
- Account Takeover Assessment

---

## Key Takeaways

This lab demonstrates that authentication security extends beyond login forms. Weakly designed persistent login cookies can expose credential material that enables complete account compromise.

It also highlights how multiple vulnerabilities, such as XSS and insecure authentication mechanisms, can be chained together to achieve a successful attack, reinforcing the importance of defense in depth.
