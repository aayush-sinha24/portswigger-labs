# SSRF with Blacklist-Based Input Filter

## Lab Overview

This lab demonstrates how blacklist-based URL filtering can be bypassed to exploit a Server-Side Request Forgery (SSRF) vulnerability.

The application attempts to block requests to localhost and the admin interface by checking for specific strings such as `127.0.0.1`, `localhost`, and `/admin`. However, because the validation relies on a blacklist instead of strict allowlisting, the filters can be bypassed using URL parsing tricks.

**Difficulty:** Practitioner

**Category:** Server-Side Request Forgery (SSRF)

---

# Objective

Bypass the application's blacklist filters, access the internal administrator interface, and delete the user **carlos**.

---

# Vulnerability

The stock check functionality accepts a user-controlled URL and performs the request from the server.

Although the application blocks obvious payloads such as:

```
http://127.0.0.1/admin
```

the filtering is based on matching strings rather than validating the actual destination after URL parsing.

As a result, multiple URL parsing techniques can bypass the blacklist.

---

# Exploitation Steps

### 1. Intercept the stock check request

Capture the request in Burp Suite.

Example:

```
stockApi=http://stock.weliketoshop.net:8080/product/stock/check?productId=1&storeId=1
```

---

### 2. Test the blacklist

Replace the URL with:

```
http://127.0.0.1/admin
```

The application blocks the request.

---

### 3. Bypass localhost filtering

Use the URL userinfo (`username@host`) syntax.

Example:

```
http://localhost:80%2523@stock.weliketoshop.net/admin
```

or

```
http://127.1/
```

depending on the filter.

In this lab, the successful payload uses URL parsing to trick the application into treating the blocked hostname as user information instead of the destination host.

---

### 4. Bypass the "/admin" filter

Double URL-encode the slash.

Example:

```
%252fadmin
```

The server decodes the payload later, allowing access to the admin endpoint.

---

### 5. Delete Carlos

Navigate to:

```
/admin/delete?username=carlos
```

The user is deleted and the lab is solved.

---

# Root Cause

The application attempts to secure outbound requests using blacklist rules.

Blacklists are unreliable because attackers can exploit:

- URL encoding
- Double encoding
- Alternative IP representations
- Username (`@`) syntax
- URL parser inconsistencies

instead of using the blocked strings directly.

---

# Security Impact

Successful exploitation may allow attackers to:

- Access internal administrative interfaces
- Reach localhost services
- Scan internal networks
- Access cloud metadata services
- Bypass network segmentation

---

# Remediation

- Use strict allowlists instead of blacklists.
- Reject private IP ranges.
- Normalize and decode URLs before validation.
- Validate the final destination after redirects.
- Restrict outbound connections to trusted hosts only.

---

# Key Takeaways

- Blacklists are not a reliable security control.
- URL parsing behavior differs across applications and libraries.
- Multiple encoding techniques can bypass naive validation.
- SSRF protection should rely on strict allowlisting and network controls rather than blocked keywords.

---

**Status:** ✅ Solved
