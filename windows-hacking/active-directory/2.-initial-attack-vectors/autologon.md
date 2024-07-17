# Autologon

> Instead of waiting for a user to enter their name and password, Windows uses the credentials you enter with Autologon, which are encrypted in the Registry, to log on the specified user automatically.

<pre class="language-powershell"><code class="lang-powershell"># get the password
<strong>reg.exe query "HKLM\software\microsoft\windows nt\currentversion\winlogon"
</strong></code></pre>

<figure><img src="../../../.gitbook/assets/image (2) (1) (1).png" alt=""><figcaption></figcaption></figure>
