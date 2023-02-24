# Tools

## `NetBot`

{% hint style="info" %}
_Simulate DDoS attack_
{% endhint %}

{% embed url="https://github.com/skavngr/netbot" %}

## `Hulk`

{% hint style="info" %}
HULK stands for **HTTP Unbearable Load King**. It is a DoS attack tool for the web server. It is created for research purposes.

**Features**:

* It can bypass the cache engine.
* It can generate unique and obscure traffic.
* It generates a great volume of traffic at the web server.

**Verdict: It may fail in hiding the identity. Traffic coming through HULK can be blocked.**
{% endhint %}

{% embed url="https://github.com/grafov/hulk" %}

## `Slowloris`&#x20;

{% hint style="info" %}
Slowloris tool is used to make a DDoS attack. It is used to make the server down.

**Features**:

* Sends authorized HTTP traffic to the server.
* Doesn’t affect other services and ports on the target network.
* This attack tries to keep the maximum connection engaged with those that are open.
* Achieves this by sending a partial request.
* Tries to hold the connections as long as possible.
* As the server keeps the false connection open, this will overflow the connection pool and will deny the request to the true connections.

**Verdict**: As it makes the attack at a slow rate, traffic can be easily detected as abnormal and can be blocked.
{% endhint %}

{% embed url="https://github.com/gkbrk/slowloris" %}

## `GoldenEye`

{% hint style="info" %}
GoldenEye is an **HTTP DoS** Test Tool. Attack Vector exploited: **HTTP Keep Alive** + **NoCache**
{% endhint %}

{% embed url="https://github.com/jseidl/GoldenEye" %}

## `Hping3`&#x20;

{% hint style="info" %}
**default in kali**

_See DDoS commands_
{% endhint %}

{% embed url="https://github.com/antirez/hping" %}
