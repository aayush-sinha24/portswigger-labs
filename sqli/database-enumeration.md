# SQL Injection Database Enumeration

## Lab Overview

This lab demonstrated how SQL injection vulnerabilities can be used to enumerate backend database structures such as tables and columns.

The application was vulnerable because user input was directly processed inside SQL queries.

---

## Objective

Use SQL injection to identify database tables, columns, and sensitive data stored inside the backend database.

---

## Vulnerability Explanation

The application trusted user-controlled input without proper sanitization.

Because SQL queries were dynamically generated using unsanitized input, it became possible to extract database metadata and enumerate internal database structures.

---

## Payload Used

```sql
' UNION SELECT table_name,NULL FROM information_schema.tables--
```

---

## Attack Process

1. Identified a vulnerable parameter.
2. Tested SQL injection behavior using basic payloads.
3. Used UNION SELECT queries to enumerate database tables.
4. Enumerated column names from discovered tables.
5. Retrieved sensitive information from backend database structures.

---

## Why The Payload Worked

The payload queried the database metadata tables available inside the database management system.

Because the application directly processed SQL syntax from user input, database structure information became accessible to the attacker.

---

## Security Impact

A successful database enumeration attack can lead to:

* Discovery of sensitive tables
* Exposure of database structure
* Credential extraction
* Sensitive information disclosure
* Easier exploitation of backend systems

---

## Mitigation

Recommended protections include:

* Parameterized queries
* Prepared statements
* Restricting database permissions
* Secure input validation
* Hiding database error information

---

## What I Learned

This lab helped me understand how attackers enumerate backend databases using SQL injection vulnerabilities and how exposed database metadata increases overall attack surface.
