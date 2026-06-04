 SQL Injection Vulnerability Allowing Login Bypass

 Lab Overview

This lab demonstrated how improper handling of user input in SQL queries can allow authentication bypass.

The application login form was vulnerable to SQL injection because user-controlled input was directly included in the backend SQL query without proper sanitization.

---

 Objective

Bypass the login functionality and access the administrator account.

---

 Vulnerability Explanation

The application failed to safely process input entered into the login form.

Because the SQL query directly included user input, it was possible to modify the query logic using special SQL characters and conditions.

This allowed authentication checks to be bypassed without knowing valid credentials.

---

## Payload Used

```sql
' OR 1=1--
```

## Attack Process
Opened the login page.
Entered the payload into the username field.
Submitted the request.
The injected condition evaluated as TRUE.
Authentication logic was bypassed and access was granted.
## Why The Payload Worked

The injected condition:

```sql
1=1
```

always evaluates to TRUE.

Because the application trusted unsanitized user input, the backend query logic was altered and the original authentication check became ineffective.

## Security Impact

A successful SQL injection vulnerability can lead to:

Authentication bypass
Unauthorized account access
Sensitive database exposure
Data modification or deletion
Full database compromise in severe cases
## Mitigation

Recommended protections include:

Prepared statements / parameterized queries
Input validation
Least-privilege database permissions
Proper error handling
Web application firewalls (WAF)
## What I Learned

This lab helped me better understand how insecure input handling can directly affect backend authentication logic and why parameterized queries are important in modern web applications.
