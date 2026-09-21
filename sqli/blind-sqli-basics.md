# Blind SQL Injection with Conditional Responses

## Objective

Exploit a blind SQL injection vulnerability by observing differences in the application's response when an injected SQL condition evaluates to true or false.

## Lab Overview

This lab demonstrates blind SQL injection using conditional responses.

The application does not directly display the result of the injected SQL query. Instead, the application behaves differently depending on whether the injected condition is true or false.

This difference can be used as a boolean information channel.

## Vulnerability

The application incorporates user-controlled input into a SQL query without sufficient parameterization.

Because the injected SQL can influence the query condition, the application's response can reveal whether a particular condition evaluated to true or false.

Conceptually:

```text
Injected condition = TRUE
        ↓
Application response A

Injected condition = FALSE
        ↓
Application response B
```

This observable difference acts as a boolean oracle.

## Exploitation Steps

### 1. Identify the injection point

Intercept the relevant request using Burp Suite and identify the parameter or cookie that is incorporated into the backend SQL query.

### 2. Establish normal behavior

Send the original request and observe the application's normal response.

This provides a baseline for comparison.

### 3. Test a true condition

Modify the input with a SQL condition that should evaluate to true.

Observe how the application responds.

### 4. Test a false condition

Modify the input with a condition that should evaluate to false.

Compare the response with the previous request.

### 5. Identify the response oracle

The difference between the true and false responses confirms that information can be inferred from application behavior without directly seeing the SQL query result.

### 6. Apply the technique to database information

Once the response behavior is understood, conditions can be constructed to test properties of values stored in the database.

For example, an attacker can test whether:

```sql
LENGTH(password) > 10
```

is true for a particular account.

The same principle can then be applied to individual character positions to infer sensitive values.

## Root Cause

The application directly incorporates untrusted input into a SQL query instead of using parameterized queries or prepared statements.

This allows an attacker to influence SQL logic and observe its effect through application behavior.

## Impact

Successful exploitation may allow an attacker to:

- Confirm SQL injection without visible database output
- Infer database values
- Extract sensitive information over multiple requests
- Recover credentials
- Potentially compromise user accounts

## Mitigation

- Use parameterized queries or prepared statements.
- Never concatenate untrusted input into SQL statements.
- Treat cookies, headers, and request parameters as untrusted input.
- Avoid exposing meaningful differences between successful and failed database conditions where practical.
- Monitor repeated requests containing suspicious SQL syntax.

## Tools Used

- Burp Suite Community Edition
- Burp Repeater
- PortSwigger Web Security Academy

## Skills Practiced

- Blind SQL injection
- Conditional SQL logic
- Boolean inference
- HTTP response comparison
- SQL injection detection
- Burp Repeater

## Key Takeaways

- Blind SQL injection does not require the database result to be displayed.
- Application behavior can act as a boolean oracle.
- True and false SQL conditions can be distinguished through response differences.
- The same technique can be extended to infer sensitive database values.
- Parameterized queries are the primary defense against SQL injection.

---

**Status:** ✅ Solved
