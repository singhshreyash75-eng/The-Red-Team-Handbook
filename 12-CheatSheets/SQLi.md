
# SQL Injection — Cheat Sheet

> SQL injection fundamentals, safe validation, database-query behavior, and defensive testing.

## 1. What is SQL Injection?

**SQL Injection (SQLi)** occurs when untrusted input is incorporated into SQL syntax without safe parameterization.

Conceptually:

```text
User Input
    ↓
Unsafe SQL Construction
    ↓
Database Parser
    ↓
Altered Query Logic
```

Possible impacts:

```text
Unauthorized data access
Authentication bypass
Data modification
Data deletion
Service disruption
```

Impact depends on database privileges and application design.

---

## 2. Vulnerable Query Pattern

Unsafe Python example:

```python
query = (
    "SELECT * FROM products WHERE id = "
    + user_input
)
```

The application constructs SQL syntax from untrusted input.

Safer approach:

```python
cursor.execute(
    "SELECT * FROM products WHERE id = ?",
    (product_id,)
)
```

This example uses SQLite parameter syntax.

---

## 3. SQLi Categories

Important categories:

```text
In-Band SQLi
Blind SQLi
Error-Based SQLi
UNION-Based SQLi
Boolean-Based SQLi
Time-Based SQLi
Second-Order SQLi
```

---

## 4. In-Band SQLi

The application returns relevant database results through the same communication channel used to submit the input.

Conceptually:

```text
Request
   ↓
Database
   ↓
Response Contains Data
```

---

## 5. Error-Based SQLi

Database errors may reveal:

```text
SQL syntax
Database engine
Table/column references
Query structure
```

Example conceptual error:

```text
SQL syntax error near ...
```

Detailed database errors should not be exposed to untrusted clients.

---

## 6. UNION-Based SQLi

SQL `UNION` combines compatible query result sets.

Normal SQL example:

```sql
SELECT name FROM current_products
UNION
SELECT name FROM archived_products;
```

Requirements include compatible column counts and data types.

In SQL injection, unsafe query construction may allow untrusted input to influence query structure.

---

## 7. Blind SQLi

Blind SQLi occurs when database behavior is observable indirectly rather than through directly returned query results.

Common categories:

```text
Boolean-Based
Time-Based
```

---

## 8. Boolean-Based SQLi

The application may produce different responses depending on whether a database condition is true or false.

Conceptually:

```text
Input A
   ↓
Condition True
   ↓
Response A
```

```text
Input B
   ↓
Condition False
   ↓
Response B
```

A response difference alone is not proof of SQL injection.

---

## 9. Time-Based SQLi

Some database operations can introduce measurable delays.

Conceptually:

```text
Input
   ↓
Database Condition
   ↓
Different Execution Time
```

Timing results can be influenced by:

```text
Network latency
Server load
Caching
Rate limiting
Database workload
```

Controlled repetition and statistical analysis are needed before drawing conclusions.

---

## 10. Second-Order SQLi

Occurs when untrusted input is stored safely at first but later used unsafely in a different SQL operation.

```text
Input
   ↓
Stored Data
   ↓
Later Query Construction
   ↓
SQL Injection
```

Defenses must be applied at every SQL execution boundary.

---

## 11. Common Injection Points

Potential input surfaces:

```text
URL query parameters
POST body
JSON fields
Form inputs
Cookies
Headers
Stored application data
```

The presence of input does not imply vulnerability.

---

## 12. Database Fingerprinting

Different databases use different SQL dialects.

Common systems:

```text
MySQL
MariaDB
PostgreSQL
Microsoft SQL Server
SQLite
Oracle
```

Database identification should be based on reliable application or administrative evidence.

---

## 13. Safe Validation Approach

In an authorized lab:

```text
Identify Input
     ↓
Understand Query Context
     ↓
Establish Baseline
     ↓
Use Non-Destructive Tests
     ↓
Compare Responses
     ↓
Verify Root Cause
```

Do not use destructive SQL statements against production data.

---

## 14. Burp Suite Workflow

```text
Capture Request
      ↓
Send to Repeater
      ↓
Identify Input
      ↓
Change One Variable
      ↓
Observe Response
      ↓
Validate Hypothesis
```

Record the original and modified requests.

---

## 15. SQLmap

**SQLmap** is an automated SQL injection testing tool.

It can assist with:

```text
Parameter analysis
Injection detection
Database fingerprinting
Controlled vulnerability validation
```

Automation can create significant traffic and database load.

Use only with explicit authorization and carefully selected options.

---

## 16. Parameterized Queries

Primary defense:

```text
Prepared Statements
+
Bound Parameters
```

Python SQLite example:

```python
cursor.execute(
    "SELECT * FROM users WHERE username = ?",
    (username,)
)
```

This separates SQL structure from parameter values.

---

## 17. PostgreSQL Example

Using a PostgreSQL driver that supports `%s` placeholders:

```python
cursor.execute(
    "SELECT * FROM users WHERE username = %s",
    (username,)
)
```

Placeholder syntax depends on the database driver.

---

## 18. Java PreparedStatement

```java
String sql =
    "SELECT * FROM users WHERE username = ?";

PreparedStatement stmt =
    connection.prepareStatement(sql);

stmt.setString(1, username);
```

This prevents the username value from being interpreted as SQL syntax.

---

## 19. Dynamic Identifiers

Bound parameters generally cannot replace SQL identifiers such as table names or column names.

For dynamic identifiers:

```text
Use an explicit allowlist
```

Example:

```python
allowed_columns = {"name", "created_at"}

if sort_column not in allowed_columns:
    raise ValueError("Invalid column")
```

Do not concatenate arbitrary user input into identifiers.

---

## 20. Input Validation

Validate:

```text
Type
Length
Format
Allowed values
Business rules
```

However:

```text
Input Validation
!=
Replacement for Parameterization
```

Both are useful for different reasons.

---

## 21. Least Privilege

Database accounts should have only required permissions.

Example:

```text
Read-Only Application
       ↓
SELECT Permission
```

Avoid unnecessary:

```text
DROP
ALTER
CREATE
Administrative permissions
```

for ordinary application accounts.

---

## 22. Error Handling

Avoid exposing detailed database errors to clients.

Prefer:

```text
Generic Client Error
       +
Detailed Protected Server Log
```

This reduces unintended information disclosure.

---

## 23. ORM Security

Object-relational mappers can reduce unsafe SQL construction, but they do not eliminate SQL injection risk.

Raw queries and unsafe string interpolation can still introduce vulnerabilities.

Always use supported parameter-binding APIs.

---

## 24. SQLi Testing Checklist

```text
[ ] Identify database-backed inputs
[ ] Establish normal response
[ ] Check parameter types
[ ] Review server-side query construction
[ ] Use non-destructive validation
[ ] Verify parameterization
[ ] Review database permissions
[ ] Check error handling
[ ] Confirm remediation
```

---

## 25. Reporting

A SQLi finding should contain:

```text
Affected endpoint
Input parameter
Database context
Observed behavior
Reproduction evidence
Security impact
Root cause
Recommended fix
```

Avoid unnecessary extraction of real data.

---

## Key Takeaway

SQL injection results from:

```text
UNTRUSTED INPUT
       +
UNSAFE SQL CONSTRUCTION
       ↓
QUERY MANIPULATION
```

The primary defense is:

```text
PARAMETERIZED QUERIES
       +
LEAST PRIVILEGE
       +
VALIDATION
       +
SAFE ERROR HANDLING
```
