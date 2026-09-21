# SQL Injection Using UNION Attacks

## Objective

Exploit a SQL injection vulnerability using a `UNION` query to retrieve additional information from the backend database.

## Lab Overview

This lab demonstrates UNION-based SQL injection.

A `UNION` query can combine the results of the original SQL query with the results of an attacker-controlled query.

When the application directly incorporates untrusted input into a SQL statement, an attacker may be able to append a second query and retrieve data that the application was not intended to expose.

## Vulnerability

The application incorporates user-controlled input into a SQL query without sufficient parameterization.

This allows additional SQL syntax to be introduced into the original query.

The general attack flow is:

```text
Identify injection point
        ↓
Determine number of columns
        ↓
Identify a suitable output column
        ↓
Construct UNION query
        ↓
Retrieve unauthorized database information
```

## Exploitation Steps

### 1. Identify the injection point

Intercept the relevant request using Burp Suite and identify a parameter that is incorporated into the backend SQL query.

### 2. Determine the number of columns

Test different `UNION SELECT` column counts until the injected query is compatible with the original query.

The number of columns in the injected query must match the number returned by the original query.

### 3. Identify columns that accept text

After determining the correct column count, test which returned columns can display string data.

This identifies the application-controlled output location that can be used to display retrieved information.

### 4. Retrieve database information

Use the identified output column with a `UNION SELECT` query to retrieve information from another table or database object.

The application then displays data that was not part of the original intended query.

### 5. Complete the lab objective

Use the retrieved information to satisfy the lab's specific objective.

## Root Cause

The application directly incorporates untrusted input into a SQL query instead of using parameterized queries or prepared statements.

Because the database accepts additional SQL syntax, the attacker can modify the original query and append a second result set.

## Impact

Successful UNION-based SQL injection may allow an attacker to:

- Retrieve unauthorized database records
- Expose credentials and secrets
- Enumerate database structures
- Access sensitive application data
- Modify or delete data when database privileges permit
- Potentially compromise the wider application

## Mitigation

- Use parameterized queries or prepared statements.
- Never concatenate untrusted input into SQL statements.
- Apply strict input validation where appropriate.
- Use least-privilege database accounts.
- Avoid exposing detailed database errors.
- Monitor requests containing suspicious SQL syntax.

## Tools Used

- Burp Suite Community Edition
- Burp Repeater
- Web Security Academy

## Skills Practiced

- UNION-based SQL injection
- SQL query manipulation
- Column-count discovery
- Output-column identification
- Database information retrieval
- HTTP request analysis
- Burp Repeater

## Key Takeaways

- UNION attacks can combine an application's original query with an attacker-controlled query.
- The injected query must have a compatible column structure.
- Identifying which columns accept textual data is an important part of UNION SQLi testing.
- UNION-based SQLi can expose data from tables that the application did not intend to expose.
- Parameterized queries are the primary defense against SQL injection.

---

**Status:** ✅ Solved
