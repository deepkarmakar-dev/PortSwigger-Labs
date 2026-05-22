# 🚨 Lab 24 — DOM XSS in jQuery Selector Sink using a Hashchange Event

## 🧪 Lab Details

| Field | Value |
|---|---|
| **Lab No** | 24 |
| **Lab Name** | DOM XSS in jQuery selector sink using a hashchange event |
| **Category** | DOM XSS |
| **Platform** | PortSwigger Web Security Academy |

---

# 🎯 Goal

Deliver an exploit that triggers JavaScript execution through the URL hash.

---

# 🔍 Injection Point

- URL Fragment (`#`)

---

# 💣 Payload

```html
<img src=x onerror=alert(1)>
```

---

# 🧠 Vulnerability Overview

The application reads user-controlled input from:

```javascript
location.hash
```

and passes it into a jQuery selector sink.

---

# ⚔️ Exploitation Steps

## 1️⃣ Inject Payload into URL Hash

Example:

```url
#<img src=x onerror=alert(1)>
```

---

## 2️⃣ Trigger `hashchange`

The page processes the hash value dynamically.

---

## 3️⃣ Browser Parses HTML

The malicious HTML gets interpreted.

---

## 4️⃣ Trigger `onerror`

Invalid image source causes:

```javascript
onerror
```

to execute.

✅ DOM XSS Successfully Triggered

---

# 🧨 Why `<script>` Often Fails

```html
<script>alert(1)</script>
```

may be treated as plain text or blocked by browser behavior.

---

# 🧨 Why `onerror` Works

HTML event handlers still execute when parsed as HTML.

---

# 🛡️ Prevention

## ✅ Avoid Passing User Input into jQuery Selectors

Never trust:

```javascript
location.hash
```

without validation.

---

## ✅ Sanitize HTML

Use safe rendering methods instead of raw HTML parsing.

---

# 💡 Key Takeaway

URL fragments (`#`) are fully attacker-controlled and commonly abused in DOM XSS vulnerabilities.
