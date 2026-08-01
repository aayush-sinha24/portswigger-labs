# Server-Side Request Forgery (SSRF)
![Platform](https://img.shields.io/badge/Platform-PortSwigger_Web_Security_Academy-orange)

![Labs](https://img.shields.io/badge/Labs-5_Completed-success)

![Status](https://img.shields.io/badge/Writeups-Complete-blue)

![Category](https://img.shields.io/badge/Vulnerability-SSRF-red)

This directory contains my write-ups for the **PortSwigger Web Security Academy** SSRF labs.

## About SSRF

Server-Side Request Forgery (SSRF) is a web vulnerability that allows an attacker to trick a vulnerable server into making unintended requests on the attacker's behalf.

Unlike a normal client-side request, the vulnerable application itself sends the request. This can allow attackers to access internal services, cloud metadata endpoints, administrative interfaces, or other resources that are not directly accessible from the Internet.

---

## Skills Practiced

- Identifying SSRF entry points
- Exploiting SSRF against localhost services
- Accessing internal back-end systems
- Bypassing blacklist-based URL filters
- Bypassing whitelist protections using URL parsing tricks
- Exploiting open redirect vulnerabilities to achieve SSRF
- Understanding blind SSRF concepts and out-of-band detection
- Understanding common SSRF mitigation techniques

---

## Completed Labs

- [x] Basic SSRF Against the Local Server
- [x] Basic SSRF Against Another Back-End System
- [x] SSRF with Blacklist-Based Input Filter
- [x] SSRF with Filter Bypass via Open Redirection
- [x] SSRF with Whitelist-Based Input Filter

---

## Skipped Labs

These labs require **Burp Suite Professional** features that are unavailable in the Community Edition.

- Blind SSRF with Out-of-Band Detection
- Blind SSRF with Shellshock Exploitation

---

## Key Takeaways

Throughout these labs I learned how attackers abuse applications that fetch user-controlled URLs to:

- Reach internal network services
- Access localhost-only functionality
- Bypass URL validation mechanisms
- Abuse redirect-based trust relationships
- Understand how SSRF impacts cloud environments

I also gained practical experience identifying SSRF entry points and understanding how real-world applications attempt to defend against SSRF attacks.

---

## References

- PortSwigger Web Security Academy
- OWASP Server-Side Request Forgery (SSRF)
