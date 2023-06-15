# Find passwords

## MimiPenguin

> Takes advantage of cleartext credentials in memory by dumping the process and extracting lines that have a high probability of containing cleartext passwords. Must have **root** permissions.

{% embed url="https://github.com/huntergregal/mimipenguin" %}

```bash
# install
git clone https://github.com/huntergregal/mimipenguin.git
cd mimipenguin/

# usage
./mimipenguin.sh
```

## Home Directories

> User's home directories can contain plaintext passwords. For example :

```bash
.netrc 
.pgpass
.bash_history
.zsh_history
.bash_history
.nano_history
.atftp_history
.mysql_history
.php_history
/root/anaconda-ks.cfg
```

## Configuration Files

**/etc** directory and subdirectories

```bash
# (system user account information)
/etc/passwd
/etc/shadow
# (MySQL configuration)
/etc/my.cnf
/etc/mysql/my.cnf
~/.my.cnf
/etc/mysql/conf.d/
/etc/mysql/mysql.conf.d/
/var/lib/mysql/mysql/user.MYD
# (PostgreSQL configuration)
/var/lib/pgsql/data/postgresql.conf 
pg_hba.conf
pg_ident.conf
# (web server configuration)
/etc/httpd/conf/httpd.conf
/etc/nginx/nginx.conf 
/var/apache2/config.inc
# (SSH server configuration)
/etc/ssh/sshd_config
# (Web Server configuration)
/etc/httpd/conf/*
/etc/nginx/*
# (Tomcat configuration)
tomcat-users.xml
find / -name 'tomcat-users.xml'
```

## Scheduled Jobs

> The cron configuration files and systemd timer units might contain scripts with embedded credentials.

```
/var/spool/cron/* 
/etc/crontab
```

## Application Files

> Custom applications might store passwords in their configuration files. Check any locations where you have custom software installed.

```bash
# web app
/var/www/html/
# other application directories
```
