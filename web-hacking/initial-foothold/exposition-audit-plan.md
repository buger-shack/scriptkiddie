# Exposition Audit - Plan

{% hint style="info" %}
The objective is to define the attack surface of a company, mainly made up of all the elements of its information system exposed on the Internet.
{% endhint %}

## Reconnaissance

* Have your target organization name
* Search through [RIPE.net](https://apps.db.ripe.net/db-web-ui/fulltextsearch) :&#x20;
  * `domain.example > person > e-mail -> GO`
  * Get these IP blocs that belongs to the company

### Subdomains find

#### Google Dorks

```txt
site:domain.example -www
```

#### Tools

**shodan**

```bash
# install
pip install shodan
# usage
shodan domain domain.example
```

**OneForAll**

```bash
git clone https://github.com/shmilylty/OneForAll.git
cd OneForAll
pip3 install -r requirements.txt

# usage
python3 oneforall.py --target domain.example run 
```

**subfinder**

```bash
# install 
go install -v github.com/projectdiscovery/subfinder/v2/cmd/subfinder@latest

# usage
subfinder -d domain.example -o domain-sub
```

## Scans

### nmap

```bash
# for each ip bloc :
blocip=0.0.0.0
filename=$(echo $blocip | tr '/' '-')
nmap -sn -v $blocip -oA ./${filename}_up --min-rate 1000
grep Up ${filename}_up.gnmap | awk '{print $2}' > ip-up-${filename}.txt
nmap -p- --open -sV -Pn -sT -v -iL ip-up-${filename}.txt -oA ./${filename}-full-scan --min-rate 1000
```

## Exploit

### gowitness

* Get a capture of each web service

```bash
gowitness file -f web.txt
gowitness report serve -a 127.0.0.1:7171
```
