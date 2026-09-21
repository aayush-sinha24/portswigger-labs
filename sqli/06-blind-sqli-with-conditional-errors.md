# Blind SQL Injection with Conditional Errors

## Objective

Exploit a blind SQL injection vulnerability that triggers a database error when a supplied condition is true, allowing information to be extracted without directly displaying the query result.

## Lab Overview

The application uses a tracking cookie in a database query. The injected SQL does not return the target data directly. Instead, a deliberate database error can be triggered conditionally.

By observing the difference between normal and error responses, it is possible to infer information about the `administrator` user's password.

## Vulnerability

The application incorporates the `TrackingId` cookie into a SQL query without sufficient parameterization.

A conditional expression can therefore be injected into the query.

The application behaves differently depending on whether the injected condition evaluates to true or false:

- Condition is true → database error occurs → HTTP 500 response
- Condition is false → query completes normally → HTTP 200 response

This difference acts as a boolean oracle.

## Exploitation Steps

### 1. Identify the injectable cookie

Intercept a request containing the `TrackingId` cookie using Burp Suite.

The cookie was used as the injection point.

### 2. Confirm conditional behavior

Inject a conditional SQL expression that intentionally produces a database error when the condition is true.

The observed HTTP response changes depending on the result of the condition.

### 3. Determine the password length

Use the database `LENGTH()` function to test different possible password lengths.

For example, test whether:

```sql
LENGTH(password) > 19
```

is true for the `administrator` account.

By comparing the HTTP status returned for different conditions, the password length was determined to be:

```text
20 characters
```

### 4. Extract the password character by character

Use the `SUBSTR()` function to test the character at a specific position.

Conceptually:

```sql
SUBSTR(password, position, 1) = 'candidate'
```

Burp Intruder can automate testing the lowercase alphanumeric character set for each position.

A response producing the database error indicates that the tested character satisfies the condition.

### 5. Repeat for all positions

Repeat the process for positions:

```text
1 → 20
```

This allows the complete password to be reconstructed one character at a time.

## Root Cause

The application directly incorporates user-controlled cookie data into a SQL query instead of using parameterized queries or prepared statements.

Because the attacker can influence the SQL expression, database errors become an observable side channel.

## Impact

An attacker may be able to:

- Extract sensitive database information
- Determine database values through conditional responses
- Recover credentials character by character
- Compromise user accounts
- Escalate the attack depending on the application's privileges

## Mitigation

- Use parameterized queries or prepared statements.
- Never concatenate user-controlled input into SQL statements.
- Treat cookies as untrusted input.
- Avoid exposing database errors to users.
- Use consistent error handling and response behavior.
- Monitor suspicious repeated requests that attempt conditional SQL expressions.

## Tools Used

- Burp Suite Community Edition
- Burp Repeater
- Burp Intruder
- Web Security Academy

## Skills Practiced

- Blind SQL injection
- Conditional SQL logic
- Error-based inference
- Password length enumeration
- Character-by-character data extraction
- Burp Intruder payload automation
- HTTP response analysis

## Key Takeaways

- Blind SQL injection does not require the application to display database query results.
- Observable differences in application behavior can act as an information oracle.
- Database errors can be used as a boolean signal when the application exposes different responses.
- Sensitive values can be extracted incrementally by testing one condition at a time.
- Parameterized queries are the primary defense against SQL injection.

---

**Status:** ✅ Solved
