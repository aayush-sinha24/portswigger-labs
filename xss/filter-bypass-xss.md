# XSS Filter Bypass Techniques

## Overview

Applications often implement weak blacklist-based filters that attackers can bypass using alternate payload constructions.

Understanding bypass techniques is critical for realistic web security testing.

---

## Key Concepts Learned

- Blacklist bypass
- Encoding techniques
- Case manipulation
- Alternate event handlers
- HTML parser abuse
- Payload obfuscation

---

## Example Payloads

```html
<ScRiPt>alert(1)</ScRiPt>
```

```html
<img src=x oNeRrOr=alert(1)>
```

```html
<svg/onload=alert(1)>
```

---

## Observations

- Blacklist filtering is unreliable.
- Browsers normalize malformed HTML aggressively.
- Multiple execution vectors exist beyond script tags.

---

## Security Impact

- Filter evasion
- WAF bypass
- Persistent browser compromise
- Increased exploit reliability

---

## Mitigation

- Whitelist validation
- Context-aware encoding
- CSP enforcement
- Secure HTML sanitization

---

## Skills Practiced

- Payload mutation
- Filter analysis
- Obfuscation techniques
- Browser parser behavior analysis
