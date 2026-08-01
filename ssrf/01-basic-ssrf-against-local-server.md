# Basic SSRF Against the Local Server

## Lab Overview

This lab demonstrates a classic Server-Side Request Forgery (SSRF) vulnerability where the application trusts a user-controlled URL and performs the request from the server itself. By abusing this behavior, an attacker can access internal services that are normally inaccessible from the public internet.

**Difficulty:** Apprentice

**Category:** Server-Side Request Forgery (SSRF)

---

# Objective

Delete the user **carlos** by exploiting an SSRF vulnerability to access the application's internal admin interface hosted on the local server.

---

# Vulnerability

The stock check functionality accepts a user-controlled URL.

Instead of validating the destination, the application fetches the supplied URL directly from the server.

Because the request originates from the server, it can access internal resources such as:

- localhost
- 127.0.0.1
- Internal admin panels

This behavior results in a Server-Side Request Forgery (SSRF).

---

# Exploitation Steps

### 1. Intercept the stock check request

Capture the request in Burp Suite and locate the vulnerable parameter.

Example:

```
stockApi=http://stock.weliketoshop.net:8080/product/stock/check?productId=1&storeId=1
```

---

### 2. Replace the URL

Modify the parameter to target the internal admin interface.

```
http://localhost/admin
```

or

```
http://127.0.0.1/admin
```

---

### 3. Send the request

Forward the modified request.

The application retrieves the internal admin page because the request originates from the server itself.

---

### 4. Delete Carlos

Locate the delete endpoint.

Example:

```
/admin/delete?username=carlos
```

Request:

```
http://localhost/admin/delete?username=carlos
```

Forward the request.

The lab is solved.

---

# Root Cause

The application performs server-side requests using user-controlled input without validating the destination URL.

As a result, attackers can force the server to access internal resources that are not publicly accessible.

---

# Security Impact

Successful exploitation can lead to:

- Access to internal administrative interfaces
- Internal network reconnaissance
- Access to cloud metadata services
- Exposure of sensitive internal APIs
- Remote attack chaining against internal systems

---

# Remediation

- Never allow arbitrary user-supplied URLs.
- Use a strict allowlist of trusted hosts.
- Block requests to localhost and private IP ranges.
- Disable unnecessary redirect following.
- Validate the final destination after redirects.
- Restrict outbound network access wherever possible.

---

# Key Takeaways

- SSRF occurs when a server fetches attacker-controlled URLs.
- Requests originating from the server can access internal resources.
- Localhost services should never be reachable through user-controlled input.
- Always validate both the requested URL and its final destination after redirects.

---

**Status:** ✅ Solved
