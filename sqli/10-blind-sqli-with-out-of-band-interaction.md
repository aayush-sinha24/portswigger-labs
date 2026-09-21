# Blind SQL Injection with Out-of-Band Interaction

## Objective

Confirm and exploit a blind SQL injection vulnerability by causing the database to make an observable out-of-band interaction.

## Lab Overview

This lab demonstrates out-of-band (OOB) blind SQL injection.

In a blind SQL injection scenario, the application's normal HTTP response may not reveal useful database information. An OOB channel provides an independent way to determine whether attacker-controlled SQL was successfully executed.

Instead of relying on the application's response, the database is induced to interact with an external, attacker-controlled service.

## Vulnerability

The application incorporates user-controlled input into a SQL query without sufficient parameterization.

The database also has the ability to make outbound network requests.

This combination allows SQL execution to be detected through an external interaction.

The attack flow is:

```text
Attacker
    ↓
Vulnerable Application
    ↓
Injected SQL
    ↓
Database
    ↓
Outbound DNS/HTTP interaction
    ↓
Attacker-controlled server
```

## Exploitation Steps

### 1. Identify the injection point

Intercept the relevant request in Burp Suite and identify the parameter or cookie that is incorporated into the SQL query.

### 2. Confirm blind behavior

Determine that the application's normal response does not directly expose useful database information.

This makes an alternative detection channel necessary.

### 3. Prepare an OOB endpoint

Generate a unique external interaction endpoint using an appropriate OOB testing mechanism.

The endpoint should be monitored for incoming DNS or HTTP interactions.

### 4. Trigger an out-of-band interaction

Modify the vulnerable input so that successful SQL execution causes the database to perform an outbound network interaction with the controlled endpoint.

The exact syntax depends on the underlying database system.

### 5. Monitor the interaction

Monitor the external endpoint for an incoming DNS or HTTP request.

Receiving the expected interaction provides evidence that the injected SQL was executed successfully.

### 6. Confirm the vulnerability

The independent interaction confirms the SQL injection even though the application's normal HTTP response does not reveal the database result.

## Root Cause

The application directly incorporates untrusted input into a SQL query rather than using parameterized queries or prepared statements.

In addition, the database environment permits outbound network communication that is unnecessary for the application's intended function.

## Impact

Successful exploitation may allow an attacker to:

- Confirm blind SQL injection
- Extract sensitive information through OOB techniques
- Establish an external communication channel
- Potentially exfiltrate database information
- Escalate the attack depending on database privileges and network access

## Mitigation

- Use parameterized queries or prepared statements.
- Never concatenate untrusted input into SQL statements.
- Restrict unnecessary outbound connectivity from database servers.
- Apply DNS and HTTP egress controls.
- Monitor unusual outbound connections from database infrastructure.
- Use least-privilege database accounts.

## Tools Used

- Burp Suite
- Burp Repeater
- Out-of-band interaction testing
- PortSwigger Web Security Academy

## Skills Practiced

- Blind SQL injection
- Out-of-band SQL injection
- OOB interaction detection
- OAST concepts
- DNS/HTTP interaction analysis
- SQL injection verification
- Network egress analysis

## Key Takeaways

- Blind SQL injection can be confirmed without relying on the application's normal response.
- Out-of-band interaction provides an independent communication channel.
- DNS or HTTP callbacks can provide evidence of database-side execution.
- Database servers should have tightly restricted outbound network access.
- Parameterized queries remain the primary defense against SQL injection.

---

**Status:** ✅ Solved
