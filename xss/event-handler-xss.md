# Event Handler XSS

## Overview

Event handler XSS abuses HTML and JavaScript event attributes to trigger malicious code execution.

Attackers leverage browser events to execute JavaScript without traditional script tags.

---

## Key Concepts Learned

- HTML event handlers
- Automatic event triggering
- Browser interaction events
- Payload execution contexts

---

## Common Event Handlers

```html
onerror
onload
onmouseover
onclick
onfocus
onmouseenter
```

---

## Example Payloads

```html
<img src=x onerror=alert(1)>
```

```html
<body onload=alert(1)>
```

```html
<input autofocus onfocus=alert(1)>
```

---

## Observations

- Event-based execution often bypasses weak filters.
- Browsers support numerous executable event contexts.
- User interaction may or may not be required.

---

## Security Impact

- Browser compromise
- Session hijacking
- User redirection
- Credential theft

---

## Mitigation

- Output encoding
- CSP enforcement
- Attribute sanitization
- Secure templating

---

## Skills Practiced

- Event-based payload crafting
- Context analysis
- Browser execution testing
