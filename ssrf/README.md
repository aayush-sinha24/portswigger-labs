# Server-Side Request Forgery (SSRF)
![Platform](https://img.shields.io/badge/Platform-PortSwigger_Web_Security_Academy-orange)

![Labs](https://img.shields.io/badge/Labs-5_Completed-success)

![Status](https://img.shields.io/badge/Writeups-Complete-blue)

![Category](https://img.shields.io/badge/Vulnerability-SSRF-red)

This directory contains my write-ups for the **PortSwigger Web Security Academy** SSRF labs.
## Table of Contents

- [About SSRF](#about-ssrf)
- [SSRF Attack Flow](#ssrf-attack-flow)
- [Skills Practiced](#skills-practiced)
- [Completed Labs](#completed-labs)
- [Skipped Labs](#skipped-labs)
- [Key Takeaways](#key-takeaways)
- [References](#references)

---

## About SSRF

Server-Side Request Forgery (SSRF) is a web vulnerability that allows an attacker to trick a vulnerable server into making unintended requests on the attacker's behalf.

Unlike a normal client-side request, the vulnerable application itself sends the request. This can allow attackers to access internal services, cloud metadata endpoints, administrative interfaces, or other resources that are not directly accessible from the Internet.
## SSRF Attack Flow

```text
                    Attacker
                        │
                        │ Provides a malicious URL
                        ▼
          Vulnerable Web Application
                        │
                        │ Server fetches the supplied URL
                        ▼
      ┌──────────────────────────────────┐
      │ Internal Resources               │
      │                                  │
      │ • localhost                      │
      │ • Internal APIs                  │
      │ • Admin Panel                    │
      │ • Database Services              │
      │ • Cloud Metadata Endpoint        │
      └──────────────────────────────────┘
                        │
                        ▼
              Sensitive Information
                        │
                        ▼
              Returned to Attacker
```

---

### Attack Flow

1. The attacker supplies a URL to the vulnerable application.
2. The application trusts the URL and sends the request.
3. The request originates from the server itself.
4. Internal resources trust the request because it comes from the server.
5. Sensitive data is returned to the attacker.

---

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

- [x] [Basic SSRF Against the Local Server](01-basic-ssrf-against-local-server.md)

- [x] [Basic SSRF Against Another Back-End System](02-basic-ssrf-against-another-back-end-system.md)

- [x] [SSRF with Blacklist-Based Input Filter](03-ssrf-with-blacklist-based-input-filter.md)

- [x] [SSRF with Filter Bypass via Open Redirection](04-ssrf-with-filter-bypass-via-open-redirection.md)

- [x] [SSRF with Whitelist-Based Input Filter](05-ssrf-with-whitelist-based-input-filter.md)

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
