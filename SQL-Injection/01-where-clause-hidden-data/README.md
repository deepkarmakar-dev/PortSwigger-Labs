# Lab 01 – SQL Injection: WHERE Clause Allows Retrieval of Hidden Data

## Lab Information
- Platform: PortSwigger Web Security Academy
- Vulnerability: SQL Injection
- Difficulty: Apprentice
- Status: Solved ✅

---

## Description
This lab contains a SQL injection vulnerability in the product category filter.
The application does not properly sanitize user input before including it in
a SQL query. As a result, an attacker can manipulate the query to retrieve
hidden or unreleased products.

---

## Vulnerable Endpoint
The SQL injection vulnerability exists in the `category` parameter
of the following URL:

```http
GET /filter?category=Gifts
