# SQL Injection Labs

This directory contains my hands-on PortSwigger Web Security Academy SQL Injection lab write-ups, techniques, observations, and defensive notes.

## Topics Covered

- Authentication bypass
- UNION-based SQL injection
- Database enumeration
- Blind SQL injection
- Conditional response inference
- Conditional error-based inference
- Visible error-based SQL injection
- Time-based blind SQL injection
- Time-based information retrieval
- Out-of-band SQL injection
- Out-of-band data exfiltration
- SQLi filter bypass using XML encoding

## Completed Write-ups

### Core SQL Injection

- [SQL Injection Vulnerability Allowing Login Bypass](./login-bypass-sqli.md)
- [SQL Injection Database Enumeration](./database-enumeration.md)
- [SQL Injection Using UNION Attacks](./union-based-sqli.md)

### Blind SQL Injection

- [Blind SQL Injection with Conditional Responses](./blind-sqli-basics.md)
- [Blind SQL Injection with Conditional Errors](./06-blind-sqli-with-conditional-errors.md)
- [Blind SQL Injection with Time Delays](./08-blind-sqli-with-time-delays.md)
- [Blind SQL Injection with Time Delays and Information Retrieval](./09-blind-sqli-with-time-delays-and-information-retrieval.md)

### Error-Based SQL Injection

- [Visible Error-Based SQL Injection](./07-visible-error-based-sqli.md)

### Out-of-Band SQL Injection

- [Blind SQL Injection with Out-of-Band Interaction](./10-blind-sqli-with-out-of-band-interaction.md)
- [Blind SQL Injection with Out-of-Band Data Exfiltration](./11-blind-sqli-with-out-of-band-data-exfiltration.md)

### Filter Bypass

- [SQL Injection with Filter Bypass via XML Encoding](./12-sqli-with-filter-bypass-via-xml-encoding.md)

## Practical Skills

- Identifying SQL injection entry points
- Manipulating backend SQL queries
- Authentication bypass testing
- UNION-based data retrieval
- Database schema enumeration
- Blind SQL injection
- Boolean and error-based inference
- Time-based inference
- Out-of-band SQL injection
- OOB data exfiltration
- Filter bypass analysis
- Burp Suite request manipulation

## Tools Used

- Burp Suite Community Edition
- Burp Repeater
- Burp Intruder
- PortSwigger Web Security Academy
- Git & GitHub

## Defensive Focus

The primary mitigation for SQL injection is the use of parameterized queries or prepared statements.

Additional defensive controls include:

- Least-privilege database accounts
- Secure error handling
- Input validation where appropriate
- Restricted database network access
- Monitoring for suspicious SQL injection activity

## Learning Outcome

These labs provided practical experience with both in-band and blind SQL injection techniques, including multiple inference channels and filter-bypass scenarios.

---

**Status:** ✅ SQL Injection module completed
