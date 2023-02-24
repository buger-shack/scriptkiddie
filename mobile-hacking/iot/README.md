# 📺 IOT

![OWASP IOT TOP 10 - 2018](<../../.gitbook/assets/image (101).png>)

### 1. Weak, Guessable or Harcoded Passwords

{% embed url="https://defpass.com/index.php" %}
Check IOT default password
{% endembed %}

{% hint style="info" %}
Use of :&#x20;

* Easily bruteforced
* Publicly available
* Unchangeable credentials

Including backdoors in firmware or client software that grants unauthorized access.
{% endhint %}

### 2. Insecure Network Services

{% hint style="info" %}
Unneeded or insecure network services running on the device itself, especially:

* Those **exposed** to the Internet
* Any that compromise the confidentiality, integrity/authenticity, or availability of information
* Any service that allows unauthorized remote control
{% endhint %}

### 3. Insecure Ecosystem Interfaces

{% hint style="info" %}
See OWASP TOP 10, insecure interfaces in the ecosystem outside the device :

* Web
* Backend API
* Cloud
* Mobile



**Common issues :**

* Lack of authentication
* Lack of authorization
* Lacking or weak encryption
* Lack of input and output filtering
{% endhint %}

### 4. Lack of Secure Update Mechanism

{% hint style="info" %}
Lack of ability to securely update the device.

* Lack of firmware validation on device
* Lack of secure delivery (un-encrypted in transit)
* Lack of anti-rollback mechanisms
* Lack of notifications of security changes due to updates
{% endhint %}

### 5. Use of Insecure or Outdated Components

{% hint style="info" %}
Use of deprecated or insecure software components/libraries that could allow the device to be compromised.

* Insecure customization of operating system platforms
* Third-party software libraries from a compromised supply chain
* Third-party hardware components from a compromised supply chain

**Examples :** _HeartBleed, Spectre, Meltdown_
{% endhint %}

### 6. Insufficient Privacy Protection

{% hint style="info" %}
User’s personal information stored on the device or in the ecosystem that is used insecurely, improperly, or without permission.

**Examples :** location, emails, addresses.
{% endhint %}

### 7. Insecure Data Transfer and Storage

{% hint style="info" %}
Lack of encryption or access control of sensitive data anywhere within the ecosystem, including at rest, in transit, or during processing.

**Examples :** lack of HSTS, no disk encryption
{% endhint %}

### 8. Lack of Device Management

{% hint style="info" %}
**Examples :** no update mechanism, no logging.
{% endhint %}

### 9. Insecure Default Settings

{% hint style="info" %}
Bad filesystem permissions&#x20;

Exposed services running as root
{% endhint %}

### 10. Lack of Physical Hardening

{% hint style="info" %}
Easily Available Debug Port Discovery
{% endhint %}

