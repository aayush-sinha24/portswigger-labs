# Blind SQL Injection with Out-of-Band Data Exfiltration

## Objective

Exploit a blind SQL injection vulnerability to extract sensitive database information through an out-of-band (OOB) interaction.

## Lab Overview

This lab builds on out-of-band SQL injection by moving beyond vulnerability confirmation.

Instead of only detecting that the database makes an external request, the injected SQL is used to include sensitive database information in the outbound interaction.

This creates a second communication channel through which data can be exfiltrated when the application's normal HTTP response does not reveal the query result.

## Vulnerability

The application incorporates user-controlled input into a SQL query without sufficient parameterization.

The database can also initiate outbound network interactions.

By combining these two weaknesses, an attacker can construct a query that causes sensitive database data to be included in a request to an attacker-controlled external server.

The attack flow is:

```text
Attacker
    ↓
Vulnerable Application
    ↓
Injected SQL
    ↓
Database retrieves sensitive data
    ↓
Database generates OOB request
    ↓
Attacker-controlled server
    ↓
Sensitive data observed
```

## Exploitation Steps

### 1. Identify the SQL injection point

Intercept the relevant request in Burp Suite and identify the parameter or cookie that is incorporated into the SQL query.

### 2. Confirm blind SQL injection

Determine that the application does not directly return useful database output.

An out-of-band interaction can be used as an alternative communication channel.

### 3. Prepare an OOB interaction endpoint

Generate a unique external interaction domain using an appropriate OOB testing mechanism.

The endpoint must be monitored for incoming DNS or HTTP interactions.

### 4. Construct the SQL injection

Modify the vulnerable input so that the database:

1. Executes the injected SQL.
2. Retrieves the required sensitive value.
3. Uses that value as part of an outbound interaction with the controlled domain.

The exact SQL syntax depends on the database management system.

### 5. Trigger the request

Send the modified request to the vulnerable application.

If successful, the database performs the outbound interaction.

### 6. Observe the OOB interaction

Monitor the external interaction service.

The recorded request contains information derived from the database query.

This allows sensitive data to be extracted even though the application's normal response does not reveal it.

### 7. Recover the target value

Analyze the recorded interaction and extract the returned database value.

The recovered information can then be used to complete the lab objective.

## Root Cause

The application is vulnerable because:

1. User-controlled input is incorporated into a SQL query.
2. The database is capable of making outbound network requests.
3. Outbound connectivity allows query results to be transmitted through an external interaction channel.

## Impact

Successful exploitation may allow an attacker to:

- Extract sensitive database information
- Recover credentials
- Exfiltrate application secrets
- Confirm blind SQL injection
- Bypass limitations of the application's normal response channel

The impact can be significant when the database has network access to external systems.

## Mitigation

- Use parameterized queries or prepared statements.
- Never concatenate untrusted input into SQL statements.
- Restrict outbound network connectivity from database servers.
- Apply DNS and HTTP egress controls.
- Monitor unusual outbound requests from database infrastructure.
- Use least-privilege database accounts.
- Prevent database servers from directly accessing untrusted external destinations.

## Tools Used

- Burp Suite
- Burp Repeater
- Out-of-band interaction testing
- Web Security Academy

## Skills Practiced

- Blind SQL injection
- Out-of-band SQL injection
- OOB data exfiltration
- OAST concepts
- Database data extraction
- DNS/HTTP interaction analysis
- SQL injection exploitation methodology

## Key Takeaways

- Blind SQL injection can be exploited even when application responses reveal no useful data.
- Out-of-band channels can be used not only to confirm SQL execution but also to exfiltrate data.
- The database's outbound network access can become an important part of the attack path.
- Restricting database egress reduces the impact of OOB SQL injection.
- Parameterized queries remain the primary defense against SQL injection.

---

**Status:** ✅ Solved
