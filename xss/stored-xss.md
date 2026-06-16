# Stored Cross-Site Scripting (XSS)

## Overview

Stored XSS occurs when malicious user input is permanently stored by the application and later rendered to other users without proper sanitization.

Unlike reflected XSS, the payload executes whenever affected content is viewed.

---

## Key Concepts Learned

- Persistent payload execution
- Server-side storage of malicious input
- Comment and profile injection
- Multi-user attack scenarios
- Browser-side script execution

---

## Example Payloads

```html
<script>alert(1)</script>
```

```html
<img src=x onerror=alert(1)>
```

```html
<svg onload=alert(1)>
```

---

## Observations

- Stored XSS is generally more dangerous than reflected XSS.
- Payloads execute automatically for every viewer.
- Applications storing unsanitized HTML are highly vulnerable.

---

## Security Impact

- Session hijacking
- Admin account compromise
- Credential theft
- Worm-like propagation
- Persistent phishing attacks

---

## Mitigation

- Output encoding
- HTML sanitization
- CSP implementation
- Input validation
- Secure rendering frameworks

---

## Skills Practiced

- Persistent payload crafting
- HTML injection analysis
- Browser behavior testing
- Context-based payload selection
