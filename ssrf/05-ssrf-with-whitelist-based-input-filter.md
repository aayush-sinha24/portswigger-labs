# SSRF with Whitelist-Based Input Filter

## Lab Overview

This lab demonstrates how whitelist-based URL validation can still be bypassed by abusing URL parsing behavior.

The application only allows requests to a trusted domain. However, by combining multiple URL parsing tricks, an attacker can make the server interpret the destination differently than the validation logic, ultimately reaching an internal administrator interface.

**Difficulty:** Expert

**Category:** Server-Side Request Forgery (SSRF)

---

# Objective

Bypass the application's whitelist validation, access the internal administrator interface, and delete the user **carlos**.

---

# Vulnerability

The stock check functionality only allows requests to a trusted hostname.

Instead of directly requesting an internal resource, the attacker abuses URL parsing features such as:

- Username (`@`) syntax
- Fragment identifiers (`#`)
- Double URL encoding
- URL parser inconsistencies

As a result, the validation logic accepts the URL while the HTTP client ultimately connects to the internal server.

---

# Exploitation Steps

### 1. Intercept the stock check request

Capture the request using Burp Suite.

Example:

```
stockApi=http://stock.weliketoshop.net:8080/product/stock/check?productId=1&storeId=1
```

---

### 2. Analyze the whitelist

The application validates that the supplied URL belongs to the trusted domain.

Direct requests such as:

```
http://localhost/admin
```

or

```
http://127.0.0.1/admin
```

are rejected.

---

### 3. Bypass the whitelist

Use URL parsing tricks to satisfy the whitelist validation while directing the HTTP client to localhost.

The successful payload combines:

- Trusted hostname
- Username (`@`)
- Fragment (`#`)
- Double URL encoding

Example payload:

```
http://localhost:80%2523@stock.weliketoshop.net/admin
```

The application validates the trusted hostname but the HTTP client ultimately connects to the internal administrator interface.

---

### 4. Delete Carlos

Access:

```
/admin/delete?username=carlos
```

The administrator endpoint becomes accessible and the user **carlos** is deleted.

---

# Root Cause

The whitelist validation checks only part of the supplied URL instead of validating the final destination after parsing.

Differences between the validation logic and the HTTP client's URL parser allow an attacker to reach internal resources.

---

# Security Impact

Successful exploitation allows attackers to:

- Bypass whitelist protections
- Access localhost services
- Reach internal administrative interfaces
- Scan internal networks
- Access cloud metadata services
- Pivot deeper into internal infrastructure

---

# Remediation

- Parse URLs before validation.
- Validate the final hostname after normalization.
- Reject private IP ranges.
- Reject redirects to internal hosts.
- Use outbound network restrictions.
- Allow requests only to explicitly approved hosts.

---

# Key Takeaways

- Whitelists are stronger than blacklists but can still fail if URL parsing is inconsistent.
- Different components may interpret the same URL differently.
- Username (`@`), fragments (`#`), encoding, and parser inconsistencies are common whitelist bypass techniques.
- Always validate the normalized, final destination rather than the raw user input.

---

**Status:** ✅ Solved
