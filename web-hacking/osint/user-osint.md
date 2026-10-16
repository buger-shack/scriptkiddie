# 🧑 User OSINT

## Everything

{% embed url="https://github.com/C0MPL3XDEV/E4GL30S1NT" %}

## Usernames

{% embed url="https://hacksnation.com/d/150-how-to-find-out-anything-about-a-person-the-best-tutorial-you-can-find" %}

{% embed url="https://app.osintracker.com/" %}

## Services:

- [OSINT Industries](https://app.osint.industries/) — Reveal what's behind any contact
- [AwareOnline : Username tools](https://www.aware-online.com/en/osint-tools/username-tools/) — Investigations, check usernames
- [Hunter](https://hunter.io/) и [Skymem](http://www.skymem.info/) — Search for corporate email addresses by URL.
- [whatsmyname](https://whatsmyname.app/) — Searches for accounts in various services by login, based on public JSON.
- [WhatsMyName Apps](https://whatsmynameapps.us/) — Free web search for a username on 3,150+ sites (WhatsMyName, Sherlock and Maigret data) with profile details and export. Also on [Android](https://play.google.com/store/apps/details?id=whatsmyname.app.us).
- [User Searcher](https://www.user-searcher.com/) — A free tool for finding a user by login on over 2 thousand websites.
- [CheckUserNames](https://checkusernames.com/), [Instant Username Search](https://instantusername.com/#/), [Namecheckr](https://www.namecheckr.com/), [peekyou](https://www.peekyou.com/username), [usersearch](https://usersearch.org/) —Online services for searching user accounts by login.

#### Social Medias

- [VKWatch](https://vk.watch) — VK profile history
- [FindClone](https://findclone.ru/) — Searching by face on social medias (VK as i remember)
- [Search4Faces](https://search4faces.com) — Search user's profile picture on social media
- [OSI.IG](https://github.com/th3unkn0n/osi.ig) — Instagram monitoring
- [WebResolser](https://webresolver.nl/) — Skype OSINT

## Utils

- [Holehe OSINT](https://github.com/megadose/holehe) — Checks if an email is associated with accounts on sites like Twitter, Instagram, and Imgur, supporting over 100 portals.
- [Mailcat](https://github.com/sharsil/mailcat) — Searches for email addresses by nickname from 22 email providers.
- [Sherlock](https://github.com/sherlock-project/sherlock) — Searches social network accounts by username.
- [Snoop Project](https://github.com/snooppr/snoop) — A login search tool covering over two and a half thousand sites, according to the developer.
- [Maigret](https://github.com/soxoj/maigret) — Collects information on a person's login, checking accounts on over two and a half thousand sites and gathering available information from web pages, without requiring API keys. (Sherlock fork)
- [Social Analyzer](https://github.com/qeeqbox/social-analyzer) — An API, command line interface, and web application for analyzing and searching profiles on over 1 thousand sites.
- [NExfil](https://github.com/thewhiteh4t/nexfil) — A Python utility for finding profiles by username on 350 websites.
- [SPY](https://github.com/CYB3R-G0D/SPY) — A fast search engine for account names, working with 210 sites.
- [Blackbird](https://github.com/p1ngul1n0/blackbird) — A tool for finding accounts by login on social networks.
- [Marple](https://github.com/soxoj/marple) — Facilitates search by login across public search engines from Google to Torch to Qwant
- [GHunt](https://github.com/mxrch/GHunt) — A modular tool for collecting data about Google accounts.
- [UserFinder](https://github.com/mishakorzik/UserFinder) — A tool for finding profiles by username.
- [Simple Email Reputation](https://emailrep.io/) — Mail verification service with some features

## Passwords

- [MD5Decrypt SHA1](https://md5decrypt.net/en/Sha1) — SHA-1  Password decryption

## HIBP API keys

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

## **Search by Phone number**

### Utils:

- [Moriarty](https://github.com/AzizKpln/Moriarty-Project) — A utility for reverse searching by phone numbers, providing information about the owner, associated links, social network pages, and other relevant details
- [Phomber](https://github.com/s41r4j/phomber) — Searches phone numbers on the internet and retrieves all available data
- [PhoneInfoga](https://github.com/sundowndev/PhoneInfoga) — A well-known tool for finding international phone numbers, providing standard information such as country, region, and carrier, and then searching for traces of it in search engines to help identify the owner.
- [kovinevmv/getcontact](https://github.com/kovinevmv/getcontact) — A utility for obtaining information from the GetContact application databases, albeit with limitations on parsing and requests.
