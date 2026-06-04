# Reflected XSS into HTML Context With Nothing Encoded

## Lab Overview

This lab demonstrated a reflected Cross-Site Scripting vulnerability caused by unsanitized user input being directly rendered inside the HTML response.

The application failed to properly encode special characters before displaying user-controlled data in the browser.

---

## Objective

Execute JavaScript in the victim's browser through the search functionality.

---

## Vulnerability Explanation

The search parameter was reflected directly into the HTML page without output encoding.

Because the application trusted user input, arbitrary JavaScript code could be injected and executed in the browser context.

This created a reflected XSS vulnerability.

---

## Payload Used

```html
<script>alert(1)</script>
```

---

## Attack Process

1. Opened the vulnerable search functionality.
2. Intercepted the reflected parameter.
3. Injected the payload into the search field.
4. Submitted the request.
5. The browser executed the injected JavaScript.

---

## Why The Payload Worked

The application returned user-controlled input directly into the HTML response without escaping special characters like:

- `<`
- `>`
- `"`

Because the payload was interpreted as actual HTML and JavaScript, the browser executed the script successfully.

---

## Security Impact

A successful reflected XSS vulnerability can lead to:

- Session hijacking
- Cookie theft
- Credential capture
- Malicious redirects
- Browser-side malware execution
- User impersonation

---

## Mitigation

Recommended protections include:

- Output encoding
- Context-aware escaping
- Input validation
- Content Security Policy (CSP)
- HttpOnly cookies
- Secure frontend frameworks

---

## What I Learned

This lab helped me understand how reflected XSS occurs when applications render untrusted input directly into HTML responses without proper sanitization or encoding.
