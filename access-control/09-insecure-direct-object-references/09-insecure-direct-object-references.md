# 🧪 Lab Writeup: Insecure Direct Object References

## 📄 Description

This lab stores user chat logs directly on the server and allows users to download them using static URLs.

The goal is to:

* Find the password of user **carlos**
* Login into his account

---

## 🚨 Vulnerability Type

Access Control → IDOR (Insecure Direct Object Reference)

---

## 🧠 What’s the Issue?

The application stores chat transcripts as files like:

/download-transcript/2.txt

The problem is:

* These files are directly accessible
* No authorization check is applied
* File names are predictable (1.txt, 2.txt, etc.)

So any user can try accessing other users' chat logs just by changing the file name.

---

## ⚔️ Steps to Exploit

### 1. Go to Live Chat

Open the live chat feature in the application.

---

### 2. Click on "View Transcript"

After ending the chat, click on **View Transcript**.

It downloads a file like:
2.txt

---

### 3. Capture Request in Burp

Intercept the request when the file is downloaded:

GET /download-transcript/2.txt

---

### 4. Modify File Name

Now change the file name to:

GET /download-transcript/1.txt

---

### 5. Check Response

Forward the request.

👉 You’ll get another user’s chat log
👉 Inside that file, you can find **carlos’s password** 🔥

---

### 6. Login as Carlos

Use the extracted password to login:

username: carlos
password: [retrieved password]

---

## ✅ Lab Solved

---

## 💥 Impact

* Unauthorized access to sensitive user data
* Exposure of credentials
* Account takeover possible
* Predictable file structure makes it easy to exploit

---

## 🛡️ How to Fix

* Enforce proper authorization checks before serving files
* Use randomized, non-guessable file names
* Store files securely (not directly accessible via URL)
* Validate user permissions before giving access

---

## 🧠 Key Learning

If file paths or IDs are predictable and not protected,
attackers can easily access other users’ data.

Always test:

* File names (1.txt, 2.txt)
* IDs
* Direct object references

---

## 🚀 Tags

#IDOR #AccessControl #CyberSecurity #BugBounty #PortSwigger
