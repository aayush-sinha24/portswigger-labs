# Blind SQL Injection Basics

## Lab Overview

This lab demonstrated how attackers can exploit SQL injection vulnerabilities even when the application does not directly display database errors or query results.

The application was vulnerable to blind SQL injection through conditional responses.

---

## Objective

Use blind SQL injection techniques to manipulate backend query behavior and identify application response differences.

---

## Vulnerability Explanation

The application processed unsanitized user input inside backend SQL queries.

Although database results were not directly visible, application behavior changed depending on whether injected conditions evaluated as TRUE or FALSE.

---

## Payload Used

```sql
' AND '1'='1
```

---

## Attack Process

1. Opened the vulnerable page.
2. Injected conditional SQL payloads into the vulnerable parameter.
3. Compared application responses for TRUE and FALSE conditions.
4. Identified response differences caused by backend query evaluation.
5. Confirmed successful blind SQL injection behavior.

---

## Why The Payload Worked

The payload introduced a conditional statement into the backend SQL query.

Because the application processed user-controlled SQL logic, response behavior changed depending on the condition result, allowing backend query behavior to be inferred indirectly.

---

## Security Impact

A successful blind SQL injection vulnerability can lead to:

* Sensitive data extraction
* Authentication bypass
* Database enumeration
* Unauthorized backend access
* Full database compromise over time

---

## Mitigation

Recommended protections include:

* Prepared statements
* Parameterized queries
* Input validation
* Secure query handling
* Limiting database permissions

---

## What I Learned

This lab helped me understand how attackers exploit SQL injection vulnerabilities even without visible database output by analyzing differences in application behavior and response patterns.
