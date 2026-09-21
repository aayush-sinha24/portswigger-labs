# SQL Injection Database Enumeration

## Objective

Exploit a SQL injection vulnerability to enumerate the structure of the backend database, including available tables and columns, and identify sensitive data stored within them.

## Lab Overview

This lab demonstrates how SQL injection can be used beyond authentication bypass or simple data retrieval.

Once a UNION-based SQL injection is identified, database metadata can be queried to discover the structure of the backend database.

Understanding this structure helps an attacker identify tables, columns, and potentially sensitive information that can be targeted in later stages.

## Vulnerability

The application incorporates user-controlled input into a SQL query without sufficient parameterization.

Because the injected SQL is executed by the database, metadata tables can be queried to enumerate the database structure.

The general attack flow is:

```text
SQL Injection
      ↓
Identify database type
      ↓
Enumerate tables
      ↓
Enumerate columns
      ↓
Identify sensitive data
      ↓
Retrieve targeted information
```

## Exploitation Steps

### 1. Identify the SQL injection point

Intercept the relevant request using Burp Suite and identify the parameter that is incorporated into the backend SQL query.

### 2. Determine the database behavior

Test the injection point and analyze the application's response to understand how the underlying SQL query behaves.

The database type and query structure influence which enumeration techniques can be used.

### 3. Enumerate database tables

Use a UNION-based SQL injection to query the database's metadata and identify available tables.

For databases that expose `information_schema`, metadata can be queried to discover table names.

### 4. Enumerate columns

After identifying the relevant table, enumerate its column names.

This reveals the structure of the table and helps identify fields that may contain sensitive information.

### 5. Identify sensitive information

Review the discovered schema and determine which columns may contain credentials, session information, personal data, or other sensitive application information.

### 6. Retrieve the required data

Use the discovered table and column structure to retrieve the information required to complete the lab objective.

## Root Cause

The application directly incorporates untrusted input into a SQL query instead of using parameterized queries or prepared statements.

This allows attackers to execute additional SQL expressions and query database metadata.

## Impact

Successful SQL injection can allow an attacker to:

- Enumerate database tables
- Discover column names and database structure
- Identify sensitive records
- Extract credentials and other secrets
- Perform further database attacks
- Potentially compromise the application and its data

## Mitigation

- Use parameterized queries or prepared statements.
- Never concatenate untrusted input into SQL statements.
- Apply least-privilege database permissions.
- Prevent application accounts from accessing unnecessary database metadata.
- Avoid exposing detailed database errors.
- Monitor suspicious SQL injection patterns in application requests.

## Tools Used

- Burp Suite Community Edition
- Burp Repeater
- Web Security Academy

## Skills Practiced

- SQL injection
- UNION-based SQL injection
- Database enumeration
- Metadata discovery
- Table enumeration
- Column enumeration
- Schema analysis
- Burp Repeater

## Key Takeaways

- SQL injection can expose much more than individual records.
- Database metadata can reveal the structure of an application's backend.
- Table and column enumeration helps identify where sensitive information is stored.
- Database privileges should be restricted to only what the application actually requires.
- Parameterized queries are the primary defense against SQL injection.

---

**Status:** ✅ Solved
