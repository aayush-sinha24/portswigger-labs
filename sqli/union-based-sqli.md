# SQL Injection Using UNION Attacks

## Lab Overview

This lab demonstrated how UNION-based SQL injection can be used to retrieve additional data from the backend database.

The application was vulnerable because user-controlled input was directly included inside a SQL query without proper sanitization.

---

## Objective

Exploit the SQL injection vulnerability to retrieve data from the database using UNION SELECT statements.

---

## Vulnerability Explanation

The application failed to properly validate user input before processing it inside backend SQL queries.

Because the database accepted additional SQL syntax from user input, it became possible to combine malicious queries with the original query using the UNION operator.

---

## Payload Used

```sql
' UNION SELECT NULL,NULL--
```

---

## Attack Process

1. Opened the vulnerable product/category page.
2. Tested input fields for SQL injection behavior.
3. Determined the correct number of columns using UNION SELECT.
4. Injected the UNION payload into the vulnerable parameter.
5. Observed successful query manipulation and retrieved additional database output.

---

## Why The Payload Worked

The UNION operator combines results from multiple SQL queries.

Because the application did not sanitize user-controlled input, the payload successfully appended a second query to the original database query.

This allowed unauthorized data retrieval from the backend database.

---

## Security Impact

A successful UNION-based SQL injection vulnerability can lead to:

* Sensitive database disclosure
* Credential leakage
* User data exposure
* Database enumeration
* Unauthorized information access

---

## Mitigation

Recommended protections include:

* Prepared statements
* Parameterized queries
* Input validation
* Least-privilege database permissions
* Secure error handling

---

## What I Learned

This lab improved my understanding of UNION-based SQL injection techniques, column enumeration, and how attackers retrieve hidden backend database information through insecure query handling.
