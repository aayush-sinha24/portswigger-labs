# SQL Injection with Filter Bypass via XML Encoding

## Objective

Exploit a SQL injection vulnerability in an XML-based request by bypassing input filtering with XML encoding.

## Lab Overview

This lab demonstrates how SQL injection filters can be bypassed when user-controlled input is processed as XML before reaching the SQL query.

The application attempts to filter SQL metacharacters and keywords from user input. However, XML entities can represent characters in an encoded form, allowing the application's filtering logic and the SQL parser to interpret the same input differently.

## Vulnerability

The application incorporates user-controlled XML input into a SQL query without using parameterized queries.

An input filter attempts to block SQL injection characters or keywords before the value reaches the database.

However, XML encoding can change how the input is represented during processing.

This creates a parser discrepancy:

```text
Attacker input
      ↓
XML parser / decoder
      ↓
Application filter
      ↓
SQL query
```

If the filtering and parsing stages interpret the input differently, malicious SQL syntax may bypass the filter and still reach the database.

## Exploitation Steps

### 1. Identify the injectable XML parameter

Intercept the relevant request in Burp Suite and identify the XML parameter that is incorporated into a database query.

### 2. Test the SQL injection

Attempt a normal SQL injection payload and observe that the application's filtering mechanism blocks the request.

### 3. Analyze the filtering behavior

Determine which characters or SQL syntax are being rejected.

The important observation is that the application processes the supplied value as XML before the database interprets it.

### 4. Encode the filtered characters using XML entities

Represent restricted characters using their XML entity equivalents.

For example, characters can be represented using XML numeric or named entities.

The XML parser decodes these entities during processing, allowing the resulting characters to reach the SQL query after the filtering stage.

### 5. Bypass the filter

Construct the SQL injection using XML-encoded characters so that:

```text
Encoded input
      ↓
Passes the filter
      ↓
XML decoding
      ↓
Original SQL characters
      ↓
SQL parser
```

This allows the SQL injection to execute despite the filtering mechanism.

### 6. Complete the lab objective

Use the resulting SQL injection to retrieve the information required by the lab and complete the challenge.

## Root Cause

The application relies on input filtering as a security control while also processing the input through an XML parser.

The filter operates on one representation of the data, while later processing converts the encoded representation into SQL syntax.

The fundamental vulnerability is the use of untrusted input in a SQL query without parameterized queries.

## Impact

Successful exploitation may allow an attacker to:

- Bypass weak SQL injection filters
- Execute arbitrary SQL expressions
- Read sensitive database information
- Modify database contents
- Potentially compromise application accounts

## Mitigation

- Use parameterized queries or prepared statements.
- Never concatenate untrusted input into SQL queries.
- Do not rely on keyword or character blacklists as the primary SQLi defense.
- Decode and canonicalize input consistently before validation when validation is required.
- Validate data according to its expected format.
- Use safe XML parsing libraries and configurations.

## Tools Used

- Burp Suite Community Edition
- Burp Repeater
- Web Security Academy

## Skills Practiced

- SQL injection
- Filter bypass
- XML encoding
- Input canonicalization
- Parser behavior analysis
- SQL injection detection
- Burp Repeater

## Key Takeaways

- Input filters are not a substitute for parameterized SQL queries.
- Encoding can create differences between how security filters and parsers interpret data.
- XML entities can be used to represent characters in an alternative form.
- Parser inconsistencies can create filter bypass opportunities.
- The strongest SQL injection defense is parameterized database access rather than blacklist-based filtering.

---

**Status:** ✅ Solved
