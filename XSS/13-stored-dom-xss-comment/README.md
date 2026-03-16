# PortSwigger Lab Write-up – Stored DOM XSS in Blog Comments

## 🧪 Lab

**Stored DOM XSS**

Platform: PortSwigger Web Security Academy

---

## 🧠 Vulnerability

**Stored DOM-Based Cross-Site Scripting (XSS)**

The blog comment functionality fetches user comments via an API and inserts them into the page using `innerHTML`. The developer attempts to sanitize user input using a custom function:

```javascript
function escapeHTML(html) {
    return html.replace('<', '&lt;').replace('>', '&gt;');
}
```

However, this sanitization is flawed because it only replaces the **first occurrence** of `<` and `>`.

Because of this incomplete escaping, an attacker can inject HTML that bypasses the filter and executes JavaScript in the browser.

---

## 🎯 Goal

Inject a malicious comment that executes JavaScript when the page loads.

---

## 🔍 Exploitation

The application displays comments using:

```javascript
commentBodyPElement.innerHTML = escapeHTML(comment.body);
```

Since `innerHTML` is used and the escaping function only replaces the first `<` and `>`, it is possible to bypass the filter by injecting additional HTML tags.

### Payload

```html
<script><img src=x onerror=alert(1)>
```

### Exploit Steps

1. Open a blog post.
2. Navigate to the **comment section**.
3. Submit a comment with the payload:

```
<script><img src=x onerror=alert(1)>
```

4. Reload the page or view the comment.

---

## 📊 Result

The browser interprets the injected HTML and triggers the following JavaScript:

```
alert(1)
```

This confirms the presence of a **Stored DOM XSS vulnerability**.

---

## 🛡 Root Cause

* Improper HTML escaping
* Unsafe use of `innerHTML`
* Custom sanitization logic that fails to properly encode user input

---

## 🔒 Mitigation

### Proper Output Encoding

Replace custom escaping with robust encoding:

```javascript
html.replace(/</g, "&lt;").replace(/>/g, "&gt;");
```

### Avoid `innerHTML`

Use safer alternatives such as:

```javascript
element.textContent = userInput;
```

### Use Trusted Libraries

Use well-tested sanitization libraries such as **DOMPurify**.

---

## 🧰 Tools Used

* Burp Suite
* Firefox
* PortSwigger Web Security Academy

---

⚠️ Disclaimer

This write-up is for educational purposes only.
All testing was performed on intentionally vulnerable applications provided by PortSwigger Web Security Academy.
