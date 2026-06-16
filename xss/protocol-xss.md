# Protocol-Based XSS

## Overview

Protocol-based XSS abuses dangerous URI schemes such as `javascript:` to trigger browser-side JavaScript execution.

Applications that improperly validate URLs may become vulnerable.

---

## Key Concepts Learned

- Dangerous URI schemes
- javascript: protocol abuse
- URL handling weaknesses
- Client-side navigation attacks
- Browser protocol parsing

---

## Dangerous Protocols Identified

```text
javascript:
data:
vbscript:
```

---

## Example Payloads

```javascript
javascript:alert(1)
```

```html
<a href="javascript:alert(1)">Click</a>
```

```html
iframe src="javascript:alert(1)"
```

---

## Observations

- URL validation failures frequently cause protocol-based XSS.
- Browser behavior differs across contexts.
- Input filtering alone is insufficient.

---

## Security Impact

- JavaScript execution
- User redirection
- Session compromise
- Phishing-style attacks

---

## Mitigation

- Allowlist safe protocols
- Reject javascript: URIs
- Sanitize user-controlled links
- Use secure rendering methods

---

## Skills Practiced

- URL injection testing
- Browser protocol analysis
- Payload crafting
- Context-aware exploitation
