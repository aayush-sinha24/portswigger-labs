# Reflected Cross-Site Scripting (XSS)

## Overview

Reflected XSS occurs when user-controlled input is immediately returned by the web application without proper sanitization or output encoding.

The malicious payload executes inside the victim's browser when the crafted request is opened.

---

## Key Concepts Learned

- HTML context injection
- Script tag injection
- Attribute-based injection
- URL parameter reflection
- JavaScript execution in browser context
- Basic payload construction
- Browser parsing behavior

---

## Example Payloads

```html
<script>alert(1)</script>
```

```html
<img src=x onerror=alert(1)>
```

```html
"><script>alert(1)</script>
```

---

## Observations

- Input reflection without sanitization leads to arbitrary JavaScript execution.
- Different contexts require different payload structures.
- Browser behavior and HTML parsing heavily affect payload execution.

---

## Security Impact

Successful reflected XSS can lead to:

- Session hijacking
- Credential theft
- Phishing attacks
- User impersonation
- Client-side malware delivery

---

## Mitigation

- Output encoding
- Context-aware escaping
- CSP (Content Security Policy)
- Input validation
- Secure templating engines

---

## Skills Practiced

- Payload crafting
- Context analysis
- Reflection point identification
- Filter behavior analysis
- Browser-side debugging
