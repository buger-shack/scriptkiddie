# ✏ Apex
## Basics
{% hint style="info" %} Oracle APEX (Application Express) is a low-code development platform that enables users to build, design, and deploy scalable and secure web applications using a web browser. It is fully integrated with the Oracle Database, making it easy to create data-driven applications quickly. APEX is particularly popular for its ease of use and the ability to create sophisticated applications with minimal coding.

Oracle APEX is based on **PL/SQL** (Procedural Language/Structured Query Language), which is Oracle's procedural extension for SQL. The platform also uses other web technologies like **HTML**, **CSS**, and **JavaScript** for the user interface and client-side functionality.

As for security, Oracle APEX is generally considered secure as it has built-in security features to protect applications from common vulnerabilities, such as SQL injection and cross-site scripting (XSS). Oracle continuously updates and enhances the platform's security measures to keep up with new threats. However, the security of an APEX application also depends on the developers' practices, like proper input validation, access control implementation, and keeping the platform up-to-date with the latest security patches. {% endhint %}

## APEX URL Synthax
```text
Application ID:Page ID:Session ID:Request:Debug:Clear Cache:Item Names:Item Values:Printer Friendly
```

APEX URL that refers to Page 1 of Application 100 : http://localhost/apex/f?p=100:1:12432087235079

### Interesting endpoints
```text
http://hostname:port/apex/apex_admin
http://hostname:port/pls/apex/apex_admin
http://hostname/ords/<workspace_name>/builder.
```

## Info Leak
Source code :
```txt
APEX_VERSION
application-version
apex-version
```
## Testing for IDOR
{% embed url="https://graytier.com/blog/f/testing-for-idor-and-authorization-vulnerabilities-in-oracle-apex" %}
### Burp Intruder
https://my.app.com/apex/f?p=x:y:SESSION:::::ITEM:ITEM_VALUE
>x = application ID
y = page ID
1. Capture a request in the proxy and send it to the Intruder tool. Set your payload position on the **pageID** parameter
2. Under **Payloads**, choose the “**Numbers**” payload and set an appropriate range you’d like to test. 
3. Run

## Testing SQLi
{% embed url="https://www.neooug.org/gloc/Presentations/2018/SpendoliniHacking%20Oracle%20APEX.pdf" %}

## sqlmap
>See slide n°24 for more infos

Rewrite with **wwv_flow.show** :
```bash
sqlmap -u "https://app.oracle.com/ords/wwv_flow.show?p_flow_id=112&p_flow_step_id=5&p_instance=14720048029141&p_arg_name=RP,45&p_arg_value=F_DISPLAY" --batch --dbms Oracle --level 3 --risk 3
```
