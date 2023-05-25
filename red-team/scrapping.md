# Scrapping People

### HIBP API

{% embed url="https://github.com/search?o=desc&p=1&q=hibp-api-key&s=indexed&type=Code" %}
Visit this and check for API keys
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
