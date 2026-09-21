# Visible Error-Based SQL Injection

## Objective

Exploit a SQL injection vulnerability that exposes verbose database error messages in the HTTP response, allowing sensitive information from the database to be extracted.

## Lab Overview

This lab demonstrates how verbose database errors can turn a SQL injection vulnerability into a direct information disclosure channel.

Instead of relying on differences in response behavior or timing, the injected query causes the database to generate an error containing data returned by the malicious query.

## Vulnerability

The application incorporates user-controlled input into a SQL query without sufficient parameterization.

The application's error handling also exposes detailed database errors to the user.

This creates an information leakage channel because an attacker can manipulate the query so that sensitive database values appear inside the resulting error message.

## Exploitation Steps

### 1. Identify the injectable parameter

Intercept the relevant request in Burp Suite and identify the parameter that is incorporated into the SQL query.

### 2. Trigger a database error

Modify the parameter to introduce an SQL expression that causes a database error.

The response reveals detailed information about the SQL query and database behavior.

### 3. Use the error message as a data channel

Construct the SQL expression so that a sensitive value is converted to an incompatible data type.

The resulting database error can contain the value returned by the injected query.

Conceptually:

```sql
CAST((SELECT password FROM users WHERE username='administrator') AS int)
```

If the database attempts to convert the returned password into an integer, the conversion error may expose the password value.

### 4. Extract the required information

Use the leaked database value from the error response to identify the administrator credentials and complete the lab objective.

## Root Cause

Two weaknesses contribute to the vulnerability:

1. User-controlled input is incorporated into a SQL query without using parameterized queries.
2. Detailed database error messages are returned to the client.

The combination allows SQL injection to become a direct data-extraction mechanism.

## Impact

An attacker may be able to:

- Extract sensitive database values
- Disclose usernames and passwords
- Reveal database structure
- Obtain application secrets
- Compromise user accounts
- Escalate the attack depending on database privileges

## Mitigation

- Use parameterized queries or prepared statements.
- Never concatenate untrusted input into SQL statements.
- Do not expose raw database errors to users.
- Implement generic application error messages.
- Log detailed database errors only on the server side.
- Apply least-privilege database permissions.

## Tools Used

- Burp Suite Community Edition
- Burp Repeater
- Web Security Academy

## Skills Practiced

- SQL injection
- Error-based SQL injection
- Verbose database error analysis
- Data extraction through error messages
- SQL type conversion behavior
- HTTP response analysis

## Key Takeaways

- Verbose database errors can become a data-exfiltration channel.
- SQL injection does not always require a visible query result.
- Database type-conversion errors can expose sensitive values.
- Detailed database errors should never be returned directly to users.
- Parameterized queries remain the primary defense against SQL injection.

---

**Status:** ✅ Solved
