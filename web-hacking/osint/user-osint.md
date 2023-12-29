# 🧑 User OSINT

## Everything

{% embed url="https://github.com/C0MPL3XDEV/E4GL30S1NT" %}

## Usernames

{% tabs %}
{% tab title="Emails" %}


{% embed url="https://hacksnation.com/d/150-how-to-find-out-anything-about-a-person-the-best-tutorial-you-can-find" %}

{% embed url="https://hunter.io" %}
emails
{% endembed %}

{% embed url="https://osint.industries" %}
emails & phones
{% endembed %}

{% embed url="https://github.com/laramies/theHarvester" %}
emails & names
{% endembed %}
{% endtab %}

{% tab title="Usernames" %}
{% embed url="https://www.aware-online.com/en/osint-tools/username-tools/" %}

{% embed url="https://whatsmyname.app/" %}
usernames
{% endembed %}

{% embed url="https://github.com/sherlock-project/sherlock" %}
usernames
{% endembed %}

```bash
git clone https://github.com/sherlock-project/sherlock.git
cd sherlock
python3 -m pip install -r requirements.txt

python3 sherlock $username
```
{% endtab %}

{% tab title="Passwords" %}
### HIBP API

{% embed url="https://github.com/search?o=desc&p=1&q=hibp-api-key&s=indexed&type=Code" %}
Visit this and check for API keys
{% endembed %}

### Pastebins

{% embed url="https://psbdmp.ws/api/search/email/" %}
add email address in url
{% endembed %}

## Get emails of company with LinkedIn

```bash
# LinkedinMama3 - https://github.com/h0useh3ad/LinkedinMama3
git clone https://github.com/h0useh3ad/LinkedinMama3.git
cd LinkedinMama3/
pip3 install -r requirements.txt

python3 LinkedinMama3.py -k $company_name -e $company_domain -n $email_format -c $linkedin_company_ID

# check if some are pwned - https://github.com/thewhiteh4t/pwnedOrNot
git clone https://github.com/thewhiteh4t/pwnedOrNot.git
cd pwnedOrNot/
chmod +x install.sh
./install.sh

nano config.json # add hibp api key
python3 pwnedornot.py -f mails-list.txt
```
{% endtab %}
{% endtabs %}
