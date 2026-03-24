## 🧪 Lab: Unprotected Admin Functionality

While testing this lab, I started by looking for hidden endpoints.  
A common place to check is the robots.txt file.

---

## 🔍 Finding the Hidden Admin Panel

I visited:

/robots.txt

There I found the following entry:

Disallow: /administrator-panel

This immediately indicated that an admin panel exists but is just hidden.

---

## 🚪 Accessing the Admin Panel

I directly navigated to:

/administrator-panel

Surprisingly, the panel was accessible without any authentication or authorization checks.

This confirms a Broken Access Control vulnerability.

---

## 💥 Exploitation

Inside the admin panel, I found an option to delete users.

The request looked like:

/administrator-panel/delete?username=carlos

I executed this request, which successfully deleted the user "carlos".

---

## ✅ Result

The lab was solved after deleting the target user.

---

## ⚠️ Impact

If this issue exists in a real-world application:

- Any user can access admin functionality
- Users can be deleted or modified
- Complete system compromise is possible

---

## 🔐 Remediation

- Enforce proper server-side authorization checks
- Restrict admin routes using role-based access control
- Do not rely on hidden paths or robots.txt for security

Example:

if ($user->role !== 'admin') {
    abort(403);
}
