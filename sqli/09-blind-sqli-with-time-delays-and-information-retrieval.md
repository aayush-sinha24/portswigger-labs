# Blind SQL Injection with Time Delays and Information Retrieval

## Objective

Exploit a blind SQL injection vulnerability by using conditional time delays to infer sensitive information from the database.

## Lab Overview

This lab builds on time-based blind SQL injection.

Instead of simply proving that SQL injection exists, the response time is used as an information channel. By making the database delay its response only when a specific condition is true, sensitive values can be reconstructed one character at a time.

## Vulnerability

The application incorporates user-controlled input into a SQL query without sufficient parameterization.

Because the application does not directly reveal the result of the injected query, the attack uses database response time as a side channel.

Conceptually:

```text
Condition is TRUE
        ↓
Database introduces delay
        ↓
Slow response

Condition is FALSE
        ↓
No delay
        ↓
Normal response
```

This allows database values to be inferred indirectly.

## Exploitation Steps

### 1. Identify the injectable parameter

Intercept the request in Burp Suite and identify the parameter or cookie that is incorporated into the SQL query.

### 2. Establish a response-time baseline

Send the normal request several times using Burp Repeater and observe the normal response time.

This provides a baseline for distinguishing delayed responses from normal network variation.

### 3. Confirm a conditional delay

Construct a SQL condition that causes a delay when the condition is true.

Conceptually:

```sql
CASE WHEN condition THEN delay ELSE no delay END
```

A clearly increased response time confirms that the condition can be used as a timing oracle.

### 4. Determine the length of the target value

Use a condition based on the `LENGTH()` function.

For example:

```sql
LENGTH(password) > 19
```

Continue testing possible lengths until the timing behavior changes.

### 5. Extract individual characters

Use a function such as `SUBSTR()` to examine one character at a time.

Conceptually:

```sql
SUBSTR(password, position, 1) = 'candidate'
```

If the condition is true, the intentional delay occurs.

### 6. Automate the process

Burp Intruder can test a character set against each position.

For each position:

```text
Position 1 → candidate characters
Position 2 → candidate characters
Position 3 → candidate characters
...
```

A delayed response identifies the character satisfying the condition.

### 7. Reconstruct the value

Repeat the process for every character position until the complete target value has been recovered.

## Root Cause

The application directly incorporates untrusted input into a SQL query instead of using parameterized queries or prepared statements.

This allows an attacker to control SQL execution and use response timing as an information disclosure channel.

## Impact

An attacker may be able to:

- Confirm SQL injection
- Determine the length of sensitive database values
- Extract credentials character by character
- Enumerate sensitive records
- Compromise application accounts

## Mitigation

- Use parameterized queries or prepared statements.
- Never concatenate user-controlled input into SQL statements.
- Avoid database-specific behavior that can act as a timing oracle.
- Apply consistent response handling.
- Monitor repeated requests with suspicious timing patterns.

## Tools Used

- Burp Suite Community Edition
- Burp Repeater
- Burp Intruder
- Web Security Academy

## Skills Practiced

- Blind SQL injection
- Time-based SQL injection
- Conditional SQL logic
- Timing side-channel analysis
- Password length enumeration
- Character-by-character data extraction
- Burp Intruder automation

## Key Takeaways

- Time-based blind SQL injection can reveal information even when no database output is visible.
- Response timing can act as a boolean oracle.
- Sensitive values can be extracted one character at a time.
- Establishing a baseline is important because normal network latency can vary.
- Parameterized queries are the primary defense against SQL injection.

---

**Status:** ✅ Solved
