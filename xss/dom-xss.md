# DOM-Based Cross-Site Scripting (DOM XSS)

## Overview

DOM XSS occurs when client-side JavaScript processes untrusted input and dynamically updates the DOM without proper sanitization.

The vulnerability exists entirely inside browser-side JavaScript logic.

---

## Key Concepts Learned

- DOM sinks
- JavaScript source-to-sink flow
- URL fragment exploitation
- document.write exploitation
- innerHTML vulnerabilities
- Client-side execution flow

---

## Dangerous Sinks Identified

```javascript
document.write()
innerHTML
eval()
setTimeout()
setInterval()
location
```

---

## Example Payloads

```html
<img src=x onerror=alert(1)>
```

```javascript
javascript:alert(1)
```

```html
"><svg/onload=alert(1)>
```

---

## Observations

- DOM XSS often bypasses server-side protections.
- Client-side JavaScript heavily influences exploitability.
- Source and sink identification is critical.

---

## Security Impact

- Session theft
- Credential capture
- Browser-side malware delivery
- Sensitive data exposure

---

## Mitigation

- Avoid dangerous sinks
- Use textContent instead of innerHTML
- Input sanitization
- CSP enforcement
- Secure JavaScript frameworks

---

## Skills Practiced

- JavaScript debugging
- DOM analysis
- Sink identification
- Client-side payload execution
