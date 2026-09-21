# Blind SQL Injection with Out-of-Band Data Exfiltration

## Objective

Exploit a blind SQL injection vulnerability to extract sensitive database information through an out-of-band (OOB) communication channel.

## Lab Overview

This lab builds on out-of-band SQL injection.

In the previous OOB lab, the goal was to confirm that the database could be induced to make an external network interaction.

Here, the OOB channel is used to carry information retrieved from the database.

The application does not directly return the required database information in its normal HTTP response, so the attacker relies on an external interaction to receive the extracted data.

## Vulnerability

The application incorporates user-controlled input into a SQL query without sufficient parameterization.

The database environment also permits outbound network communication.

This creates a path where SQL execution can retrieve sensitive information and transmit it through an external interaction channel.

The attack flow is:

```text
Attacker
    ↓
Vulnerable Application
    ↓
Injected SQL
    ↓
Database retrieves target information
    ↓
Data included in OOB interaction
    ↓
Attacker-controlled endpoint
    ↓
Sensitive information observed
```

## Exploitation Steps

### 1. Identify the SQL injection point

Intercept the relevant request using Burp Suite and identify the parameter or cookie that is incorporated into the SQL query.

### 2. Confirm blind behavior

Verify that the application's normal response does not directly reveal the required database information.

### 3. Prepare an OOB interaction endpoint

Create a unique external endpoint that can record DNS or HTTP interactions.

The endpoint is used to receive the database-generated request.

### 4. Construct the OOB data-extraction request

Modify the vulnerable input so that the database retrieves the required value and incorporates it into the outbound interaction.

The exact syntax depends on the database management system and the OOB mechanism being used.

### 5. Trigger the request

Send the modified request to the application.

The database processes the injected SQL and, if successful, generates an outbound interaction.

### 6. Analyze the recorded interaction

Inspect the interaction received by the external endpoint.

The recorded request contains information derived from the database query.

This allows information to be recovered even though it was not present in the application's normal HTTP response.

### 7. Complete the lab

Use the recovered database information to satisfy the lab's objective.

## Root Cause

The vulnerability results from the combination of:

1. User-controlled input being incorporated into a SQL query.
2. Lack of parameterized queries or prepared statements.
3. Unnecessary outbound network connectivity from the database environment.

Together, these weaknesses allow database information to be transmitted through an external communication channel.

## Impact

Successful exploitation may allow an attacker to:

- Extract sensitive database information
- Recover credentials or secrets
- Exfiltrate data through an external channel
- Bypass limitations of the application's normal response
- Escalate the attack depending on database privileges

## Mitigation

- Use parameterized queries or prepared statements.
- Never concatenate untrusted input into SQL statements.
- Restrict unnecessary outbound network connectivity from database systems.
- Apply DNS and HTTP egress controls.
- Monitor unusual outbound requests originating from database infrastructure.
- Use least-privilege database accounts.
- Monitor repeated requests that indicate automated SQL injection attempts.

## Tools Used

- Burp Suite
- Burp Repeater
- Out-of-band interaction testing
- PortSwigger Web Security Academy

## Skills Practiced

- Blind SQL injection
- Out-of-band SQL injection
- OOB data exfiltration concepts
- OAST
- Database information extraction
- DNS/HTTP interaction analysis
- Network egress analysis

## Key Takeaways

- Blind SQL injection can be exploited even when the application does not reveal database results.
- OOB techniques can provide both vulnerability confirmation and a channel for data extraction.
- Database outbound connectivity can significantly increase the impact of SQL injection.
- Sensitive database information should never be allowed to leave through unnecessary network paths.
- Parameterized queries are the primary defense against SQL injection.

---

**Status:** ✅ Solved
