# SQL Injection Vulnerability Allowing Login Bypass

## Objective

Exploit a SQL injection vulnerability in the application's authentication mechanism to bypass the login process and gain access without valid credentials.

## Lab Overview

This lab demonstrates how SQL injection can directly affect authentication logic.

The application constructs a SQL query using user-controlled login input. Because the input is not safely parameterized, SQL syntax can alter the intended authentication query.

## Vulnerability

The application incorporates untrusted username input into a SQL query without using parameterized queries or prepared statements.

This allows an attacker to modify the logic of the authentication query.

Conceptually:

```text
User-supplied input
        ↓
SQL query construction
        ↓
Modified query logic
        ↓
Authentication check altered
        ↓
Unauthorized access
```

## Exploitation Steps

### 1. Identify the login functionality

Open the application's login page and inspect the username and password parameters.

### 2. Test for SQL injection

Submit controlled SQL syntax through the username field and observe whether the application's authentication behavior changes.

### 3. Analyze the query behavior

Determine whether the supplied input can alter the logical conditions used by the backend authentication query.

A condition that always evaluates to true can interfere with the application's intended credential check.

### 4. Bypass authentication

Use the identified SQL injection behavior to modify the authentication query and bypass the normal password verification.

The lab can be completed when unauthorized access to the target account is obtained.

## Root Cause

The application directly incorporates user-controlled login data into a SQL query without parameterized database access.

This allows the attacker to influence the SQL statement executed by the database.

## Impact

Successful exploitation may allow an attacker to:

- Bypass authentication
- Access accounts without valid credentials
- Access administrative functionality
- Retrieve sensitive application information
- Potentially compromise the database

## Mitigation

- Use parameterized queries or prepared statements.
- Never concatenate user-controlled input into SQL statements.
- Treat all authentication parameters as untrusted input.
- Use least-privilege database accounts.
- Implement secure error handling.
- Log and monitor suspicious authentication requests.

## Tools Used

- Burp Suite Community Edition
- Burp Repeater
- Web Security Academy

## Skills Practiced

- SQL injection
- Authentication bypass
- SQL query manipulation
- Boolean SQL logic
- Login-flow testing
- Burp Repeater
- HTTP request analysis

## Key Takeaways

- SQL injection can directly compromise authentication mechanisms.
- Login forms are important SQL injection testing targets.
- Manipulating SQL query logic can bypass application-level authentication checks.
- Authentication controls should never rely on dynamically concatenated SQL.
- Parameterized queries are the primary defense against SQL injection.

---

**Status:** ✅ Solved
