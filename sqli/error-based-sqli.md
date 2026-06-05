# Error-Based SQL Injection

## Lab Overview

This lab demonstrated how database error messages can reveal sensitive information during SQL injection attacks.

The application exposed backend database behavior through verbose error responses.

---

## Objective

Exploit the SQL injection vulnerability and use database error responses to gather backend information.

---

## Vulnerability Explanation

The application failed to properly sanitize user input before including it in SQL queries.

Because detailed database errors were returned to the user, attackers could use malformed SQL syntax to extract information about the backend database structure and query behavior.

---

## Payload Used

```sql
' OR 1=1--
```

---

## Attack Process

1. Opened the vulnerable page.
2. Injected SQL payloads into the vulnerable parameter.
3. Observed database-generated error messages.
4. Analyzed error responses to understand backend query behavior.
5. Successfully manipulated application logic through SQL injection.

---

## Why The Payload Worked

The application exposed internal database error information instead of securely handling invalid queries.

Because user input directly affected the backend SQL query, malicious SQL syntax altered the query logic and triggered useful database responses.

---

## Security Impact

A successful error-based SQL injection vulnerability can lead to:

* Database information disclosure
* Exposure of table and column names
* Authentication bypass
* Sensitive data leakage
* Backend database compromise

---

## Mitigation

Recommended protections include:

* Parameterized queries
* Prepared statements
* Generic error handling
* Input validation
* Limiting database permissions

---

## What I Learned

This lab improved my understanding of how verbose database error messages assist attackers during SQL injection exploitation and why secure error handling is critical in modern applications.
