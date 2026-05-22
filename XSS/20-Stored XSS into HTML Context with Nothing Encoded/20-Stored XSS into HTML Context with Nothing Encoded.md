# 🚨 Stored XSS into HTML Context with Nothing Encoded

## 🧪 Lab Details

| Field | Value |
|---|---|
| **Lab Name** | Stored XSS into HTML context with nothing encoded |
| **Difficulty** | Apprentice |
| **Platform** | PortSwigger Web Security Academy |
| **Category** | Stored Cross-Site Scripting (Stored XSS) |

---

# 🎯 Goal

Submit a malicious comment that executes JavaScript when the blog post is viewed.

---

# 🔍 Injection Point

## Comment Functionality

The application stores user comments and later displays them without sanitization.

---

# 🧠 Context Analysis

| Parameter | Value |
|---|---|
| Vulnerability Type | Stored XSS |
| Injection Point | Comment Field |
| Context | HTML Body |
| Encoding | None |
| Persistence | Stored in Database |

---

# 💣 Payload

```html
<script>alert(1)</script>
```

---

# ⚔️ Exploitation Steps

## 1️⃣ Open the Blog Post

Navigate to any blog post containing a comment section.

---

## 2️⃣ Submit the Payload

Insert the following payload into the comment field:

```html
<script>alert(1)</script>
```

---

## 3️⃣ Post the Comment

The application stores the payload in the database.

---

## 4️⃣ Reload/View the Blog Post

When the blog page loads, the stored payload is rendered into the HTML response.

---

## 5️⃣ JavaScript Execution

The browser interprets the payload as executable JavaScript:

```javascript
alert(1)
```

✅ Stored XSS Successfully Triggered

---

# 📊 Result

| Test | Status |
|---|---|
| Payload Stored | ✅ |
| Input Sanitized | ❌ |
| Script Execution | ✅ |
| Persistent Attack | ✅ |

---

# 🧨 Why It Works

- User input is stored directly in the database
- No sanitization or output encoding is applied
- Stored content is rendered inside the HTML body
- Browser executes injected JavaScript automatically

---

# 🔍 Root Cause

## ❌ Unsafe Storage of User Input

The application stores attacker-controlled input without validation.

---

## ❌ Missing Output Encoding

Special characters are rendered directly into the page.

---

## ❌ Insecure Rendering

The application likely uses unsafe methods such as:

```javascript
element.innerHTML = comment;
```

instead of safe rendering methods.

---

# 🛡️ Mitigation

## ✅ 1. Validate & Sanitize Input

Remove dangerous HTML and script content before storing data.

---

## ✅ 2. Encode Output

Encode user-controlled data before rendering:

```javascript
html
  .replace(/</g, "&lt;")
  .replace(/>/g, "&gt;");
```

---

## ✅ 3. Use Safe DOM APIs

Prefer:

```javascript
element.textContent = userInput;
```

instead of:

```javascript
element.innerHTML = userInput;
```

---

## ✅ 4. Implement Content Security Policy (CSP)

```http
Content-Security-Policy: script-src 'self'
```

This reduces XSS exploitation impact.

---

# 📚 Learning

This lab demonstrates how dangerous stored user input can become when rendered without sanitization.

Unlike reflected XSS, stored XSS:

- Persists in the database
- Impacts multiple users
- Executes automatically when content is viewed

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

Stored XSS becomes critical when attacker-controlled input is permanently stored and rendered without sanitization.

Always sanitize and encode user-controlled data before rendering it into HTML.
