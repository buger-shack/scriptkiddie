# File sharing

## lightweight Web Server

```bash
# python
python -m SimpleHTTPServer 8080
python3 -m http.server

# busybox
busybox httpd --help # show available options
busybox httpd -p 127.0.0.1:8080 -h /var/www/  # start start httpd 
pkill busybox  # to stop busybo httpd

# npm
http-server

# php
php -S 127.0.0.1:8080
```

## Others

```bash
# curl
curl -O http://192.168.0.101/file.txt

# nc
# to share
nc -lvp 4444 < file
# to receive
nc 192.168.1.102 4444 > file

# scp
scp /path/to/source/file.ext username@192.168.1.101:/path/to/destination/file.ext
```
