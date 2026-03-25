# 📝 Lab Write-up: User ID controlled by request parameter

## 🎯 Objective

The goal of this lab is to exploit a horizontal privilege escalation vulnerability to obtain the API key of the user **carlos** and submit it as the solution.

---

## 🔐 Step 1: Login

I started by logging into the application using the provided credentials:

* Username: wiener
* Password: peter

After successful authentication, I navigated to the account page.

---

## 🔍 Step 2: Analyze the URL

On the account page, I observed the following URL:

```
/my-account?id=wiener
```

This indicates that the application is using a request parameter (`id`) to determine which user's data to display.

This raised a suspicion of a potential IDOR (Insecure Direct Object Reference) vulnerability.

---

## 🧪 Step 3: Parameter Manipulation

To test this, I modified the `id` parameter in the URL:

```
/my-account?id=carlos
```

After loading the modified URL, the application displayed the account details of the user **carlos**.

---

## 💥 Step 4: Extract Sensitive Information

On the page, I was able to view sensitive information belonging to **carlos**, including the API key.

This confirmed that the application does not properly enforce authorization checks.

---

## ⚡ Step 5: Submit the Solution

I copied the API key of **carlos** and submitted it in the lab solution field.

The lab was successfully solved.

---

## 🧠 Root Cause

The application trusts user-controlled input (`id` parameter) without verifying whether the requested resource belongs to the authenticated user.

There is no server-side authorization check to restrict access to only the logged-in user's data.

---

## ⚠️ Vulnerability Type

* Broken Access Control
* Insecure Direct Object Reference (IDOR)
* Horizontal Privilege Escalation

---

## 🛡️ Remediation

To fix this issue, the application should:

* Avoid using user-controlled identifiers directly for accessing sensitive data
* Enforce strict server-side authorization checks
* Ensure that users can only access their own resources

Example (secure approach):

```php
$user = auth()->user();
```

---

## 🔥 Impact

An attacker can access sensitive data of other users, including API keys and personal information. This could lead to account takeover and further exploitation.

---

## ✅ Conclusion

By manipulating the `id` parameter in the request, I was able to access another user’s account data and retrieve their API key. This demonstrates a classic IDOR vulnerability caused by missing authorization checks.
