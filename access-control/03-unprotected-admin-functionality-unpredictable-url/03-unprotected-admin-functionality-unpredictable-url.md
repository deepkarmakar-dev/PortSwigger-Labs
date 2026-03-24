## 🧪 Lab: Unprotected Admin Functionality with Unpredictable URL

Platform: PortSwigger Web Security Academy  
Category: Access Control  
Level: Apprentice  

---

## 📌 Lab Description

This lab has an admin panel at a hidden/unpredictable URL.  
Goal is to find the admin panel and delete user "carlos".

---

## 🔍 Vulnerability

Broken Access Control  

Admin panel is only hidden (not protected). No proper server-side check.

---

## 🔎 Steps to Solve

1. Page source / JS files check karo  
2. Hidden URL mila:
   /admin-54edq0  

3. Direct open karo:
   /admin-54edq0  

4. Admin panel access ho jayega  

5. User delete karo:
   /admin-54edq0/delete?username=carlos  

---

## ✅ Result

User "carlos" delete ho gaya aur lab solve ho gaya

---

## ⚠️ Impact

- Unauthorized admin access  
- Users delete/modify ho sakte hain  
- Full system compromise possible  

---

## 🔐 Fix

- Server-side access control lagao  
- Hidden URL pe trust mat karo  

Example:
if ($user->role !== 'admin') {
    abort(403);
}
