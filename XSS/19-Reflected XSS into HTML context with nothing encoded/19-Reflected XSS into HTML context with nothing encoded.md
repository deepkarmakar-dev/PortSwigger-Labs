🚨 Reflected XSS into HTML Context with Nothing Encoded

🧪 Lab Details

Lab Name: Reflected XSS into HTML context with nothing encoded

Difficulty: Apprentice

Platform: PortSwigger Web Security Academy

Category: Reflected Cross-Site Scripting (XSS)



---

🎯 Goal

Identify a reflected XSS vulnerability in the search functionality and execute JavaScript using:

<script>alert(1)</script>


---

🔍 Injection Point

Search Functionality

The search parameter reflects user input directly into the page response.


---

🧠 Context Analysis

Parameter	Value

Reflection Type	Reflected
Context	HTML Body
Encoding	None
User Interaction Required	❌


The application inserts user input into the HTML body without encoding or sanitization.


---

💣 Working Payload

<script>alert(1)</script>


---

⚔️ Exploitation Steps

1. Open the Lab

Navigate to the vulnerable search page.


---

2. Locate the Search Input

Find the search functionality on the webpage.


---

3. Inject the Payload

Enter the following payload:

<script>alert(1)</script>


---

4. Submit the Request

After submission, the payload is reflected into the HTML response.


---

5. JavaScript Executes

The browser parses the injected <script> tag and executes:

alert(1)

✅ Reflected XSS confirmed.


---

📊 Result

Test	Status

Input Reflection	✅
HTML Encoding Present	❌
Script Execution	✅
XSS Vulnerability	✅



---

🧨 Why It Works

User input is reflected directly into the HTML body

No sanitization or output encoding is applied

Browser interprets injected content as executable HTML/JavaScript



---

🔍 Root Cause

Unsafe Reflection of User Input

The application places attacker-controlled input directly into the page response.


---

Missing Output Encoding

Special characters like:

<
>
"
'

are not encoded before rendering.


---

Insecure DOM Handling

The application likely uses unsafe rendering methods such as:

element.innerHTML = userInput;


---

🛡️ Prevention

1. Output Encoding

Encode user input before rendering:

html
  .replace(/</g, "&lt;")
  .replace(/>/g, "&gt;");


---

2. Use Safe DOM APIs

Prefer:

element.textContent = userInput;

instead of:

element.innerHTML = userInput;


---

3. Input Sanitization

Use libraries like:

DOMPurify


to sanitize untrusted HTML.


---

4. Implement CSP

Example:

Content-Security-Policy: script-src 'self'

This reduces XSS impact significantly.


---

📚 Learning

This lab demonstrates how dangerous direct HTML reflection can be when no encoding is applied.

Even a basic payload like:

<script>alert(1)</script>

can lead to full JavaScript execution.


---

🧰 Tools Used

Burp Suite

Browser DevTools

PortSwigger Web Security Academy



---

⚠️ Disclaimer

This write-up is for educational purposes only.

All testing was performed on intentionally vulnerable lab environments.


---

💡 Key Takeaway

If user input is reflected into HTML without encoding, XSS becomes trivial.

Never trust user-controlled data inside HTML contexts.
