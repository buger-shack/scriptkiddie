# 🍯 HoneyPots

![](<../.gitbook/assets/giphy (1).gif>)

## CanaryTokens

{% hint style="info" %}
**Canary tokens are a free, quick, painless way to help defenders discover they've been breached (by having attackers announce themselves.)**\\

**How tokens works (in 3 short steps):**

1. Visit the site and get a free token (which could look like an URL or a hostname, depending on your selection.)
2. If an attacker ever uses the token somehow, we will give you an out of band (email or sms) notification that it's been visited.
3. As an added bonus, we give you a bunch of hints and tools that increase the likelihood of an attacker tripping on a canary token.

**More Details:**

Tokens consist of a unique identifier (which can be embedded in either HTTP URLs or in hostnames.) Whenever that URL is requested, or the hostname is resolved, we send a notification email to the address tied to the token. You can get one in seconds, using just your browser.
{% endhint %}

{% embed url="https://canarytokens.org/generate" %}
Generate CanaryTokens
{% endembed %}

{% embed url="https://www.youtube.com/watch?v=gnajUKkYy5U" %}
CanaryTokens Tutorial
{% endembed %}

{% embed url="https://github.com/thinkst/canarytokens" %}

## Pixel That Steals Data - I’m Invisible

A vulnerability using which an attacker can obtain the information of all the users without their knowledge. He can steal his IP address, ISP, country name, city name, region, Device info, browser details.

This vulnerability can be found on the places where you have an option of uploading an image using URL eg. forums, discussion pages, comments sections, messages, fetching image using `<img src=”URL”>` tag etc.

### How to

1. Go to [IPLogger](https://iplogger.org/invisible/) and generate an invisible image
2. After that a link will be generated, copy it and click on Logged IP’s
3. Now upload the image : 2 ways
   * Fetch image using web
   * Fetch image using `<img src=”URL”>` tag
