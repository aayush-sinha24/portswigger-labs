# Blind SQL Injection with Time Delays

## Objective

Exploit a blind SQL injection vulnerability where the result of an injected condition is not directly reflected in the response. Instead, a database time delay is used as an observable side channel.

## Lab Overview

This lab demonstrates time-based blind SQL injection.

The application does not display database errors or the results of the injected query. However, an injected SQL expression can deliberately introduce a delay in the database response.

By comparing response times, it is possible to determine whether the injected condition was executed successfully.

## Vulnerability

The application incorporates user-controlled input into a SQL query without sufficient parameterization.

Because the application does not directly reveal the query result, the attack relies on the database response time as an inference mechanism.

Conceptually:

```text
Condition is false
        ↓
Normal response time

Condition is true
        ↓
Intentional database delay
        ↓
Noticeably slower response
```

This creates a time-based information channel.

## Exploitation Steps

### 1. Identify the injectable parameter

Intercept the relevant request in Burp Suite and identify the parameter or cookie that is incorporated into the SQL query.

### 2. Establish a baseline

Send the normal request several times using Burp Repeater and observe the usual response time.

This provides a baseline for comparison.

### 3. Introduce a conditional delay

Modify the input so that the database performs a time-consuming operation when a chosen condition is true.

For example, depending on the database:

```sql
CASE WHEN condition THEN delay ELSE no delay END
```

The exact delay function depends on the underlying database engine.

### 4. Compare response times

Send requests with different conditions.

A substantially longer response indicates that the condition evaluated as true and the delay was triggered.

A normal response indicates that the condition evaluated as false.

### 5. Use the delay as an oracle

The response time can now be treated as a boolean signal:

```text
Delayed response → TRUE
Normal response  → FALSE
```

This allows information to be inferred even though the database result is not displayed.

## Root Cause

The application directly incorporates untrusted input into a SQL query without using parameterized queries or prepared statements.

Because the injected SQL can influence database execution time, timing differences become an information disclosure channel.

## Impact

An attacker may be able to:

- Confirm SQL injection without visible query results
- Infer database information through response timing
- Extract sensitive values character by character
- Enumerate database contents
- Potentially compromise application accounts

## Mitigation

- Use parameterized queries or prepared statements.
- Never concatenate untrusted input into SQL statements.
- Use consistent query execution paths where possible.
- Avoid exposing database-specific behavior through application responses.
- Monitor repeated requests that generate suspicious timing patterns.

## Tools Used

- Burp Suite Community Edition
- Burp Repeater
- Web Security Academy

## Skills Practiced

- Blind SQL injection
- Time-based SQL injection
- Response-time analysis
- Conditional SQL logic
- SQL injection detection
- Burp Repeater
- Timing side-channel analysis

## Key Takeaways

- Blind SQL injection can be exploited even when query results are not visible.
- Database execution time can act as an information oracle.
- A noticeable delay can indicate that a supplied SQL condition evaluated as true.
- Timing-based techniques are especially useful when error messages and query output are suppressed.
- Parameterized queries are the primary defense against SQL injection.

---

**Status:** ✅ Solved
