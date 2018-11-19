# Debian 9 install cheatsheet

## sudo
https://www.geek17.com/fr/content/debian-9-stretch-installer-et-configurer-sudo-61

```bash
su
apt install sudo
adduser spout sudo
```

## Update
```bash
sudo apt update
sudo apt upgrade
```

## byobu
```bash
sudo apt install byobu
byobu
```

## SSH
```bash
sudo nano /etc/ssh/sshd_config
Port 10022

sudo service ssh restart
```

## Firewall
https://www.digitalocean.com/community/tutorials/how-to-setup-a-firewall-with-ufw-on-an-ubuntu-and-debian-cloud-server

```bash
sudo apt install ufw

sudo nano /etc/default/ufw
IPV6=no

sudo ufw disable
sudo ufw enable

sudo ufw default deny incoming
sudo ufw default allow outgoing

# ufw allow ssh
sudo ufw allow 10022
sudo ufw allow http

sudo ufw show added
sudo ufw enable
sudo ufw status
```

## fail2ban
```bash
sudo apt install fail2ban
sudo nano /etc/fail2ban/jail.conf

destemail = votremail@domain.com
action = %(action_mwl)s

# action_ => simple ban
# action_mw => ban et envoi de mail
# action_mwl => ban, envoi de mail accompagné des logs

sudo service fail2ban restart
```

## Mail
```bash
sudo apt install exim4-config
sudo dpkg-reconfigure exim4-config
```

1. internet site; mail is sent and received directly using SMTP
2. System mail name: ENTER
3. IP-addresses: ENTER
4. Other destinations: ENTER
5. Domains to relay mail for: ENTER
6. Machines to relay mail for: ENTER
7. Keep number of DNS-queries minimal: NO
8. Delivery method: mbox format
9. Split configuration into small files: NO
10. Root and postmaster mail recipient: ENTER

## DEB.SURY.ORG
https://deb.sury.org/

```bash
sudo apt-get -y install apt-transport-https lsb-release ca-certificates
sudo wget -O /etc/apt/trusted.gpg.d/php.gpg https://packages.sury.org/php/apt.gpg
sudo sh -c 'echo "deb https://packages.sury.org/php/ $(lsb_release -sc) main" > /etc/apt/sources.list.d/php.list'
sudo apt-get update
```

## MariaDB
https://www.geek17.com/fr/content/debian-9-stretch-installer-et-configurer-mariadb-65
https://www.digitalocean.com/community/tutorials/how-to-install-mariadb-on-debian-9

```bash
sudo apt install mariadb-server
sudo mysql_secure_installation
```

```bash
sudo mysql -u root -p
```

```sql
UPDATE user SET plugin='' WHERE user='root';
FLUSH PRIVILEGES;
EXIT;
```

```bash
mysql -u root -p
```

## PHP
```bash
sudo apt install php7.2-fpm php7.2-gd php7.2-mysql php7.2-pgsql php7.2-sqlite3 php7.2-mbstring php7.2-xml php7.2-intl
```

## nginx
```bash
sudo apt install nginx

sudo nano /etc/nginx/sites-available/default

root /var/www;
index index.php index.html index.htm

# Uncomment location ~\.php$ {
# Uncomment include snippets/fastcgi-php.conf;
# Uncomment and change (PHP7) fastcgi_pass unix:/var/run/php/php7.2-fpm.sock;

sudo service nginx reload

sudo chown www-data:www-data /var/www
sudo chmod g+w /var/www

# Gzip
sudo nano /etc/nginx/nginx.conf
# Uncomment:
gzip_vary on;
gzip_proxied any;
gzip_comp_level 6;
gzip_buffers 16 8k;
gzip_http_version 1.1;
gzip_types text/plain text/css application/json application/x-javascript text/xml application/xml application/xml+rss text/javascript;

sudo nano /etc/nginx/nginx.conf
# Uncomment:
server_tokens off;

sudo service nginx reload
```

## Locales
```bash
sudo dpkg-reconfigure locales

# fr_FR.UTF-8
# nl_NL.UTF-8

locale -a
```

## Gettext
```bash
sudo apt install gettext
```

## Redis
```bash
sudo apt install redis-server
```

## ClamAV
```bash
sudo apt install clamav clamav-freshclam
```

## Adminer
```bash
sudo nano /usr/bin/adminer-update

#!/bin/bash
wget -O /var/www/adminer.php https://www.adminer.org/latest.php

sudo chmod +x /usr/bin/adminer-update
sudo adminer-update
```

## CURL
```bash
sudo apt install curl
```

## pip
```bash
curl https://bootstrap.pypa.io/get-pip.py -o get-pip.py
sudo python get-pip.py
```

## Pipenv
```bash
pip install --user pipenv

nano ~/.profile
export PATH="$PATH:~/.local/bin"

source ~/.profile
```

## ZIP
```bash
sudo apt install zip unzip
```

## Git
```bash
sudo apt install git
```

## OptiPNG
```bash
wget http://downloads.sourceforge.net/project/optipng/OptiPNG/optipng-0.7.7/optipng-0.7.7.tar.gz
tar -xvzf optipng-0.7.7.tar.gz
cd optipng-0.7.7
./configure
make
sudo make install
```

## Jpegoptim
```bash
sudo apt install libjpeg-dev

wget https://www.kokkonen.net/tjko/src/jpegoptim-1.4.6.tar.gz
tar -xvzf jpegoptim-1.4.6.tar.gz
cd jpegoptim-1.4.6
./configure
make
sudo make install
```

## TeamSpeak
https://www.vultr.com/docs/how-to-install-teamspeak-3-server-on-debian-9-stretch

```bash
sudo adduser --disabled-login teamspeak
sudo su teamspeak
cd
wget http://dl.4players.de/ts/releases/3.5.0/teamspeak3-server_linux_amd64-3.5.0.tar.bz2
tar xvf teamspeak3-server_linux_amd64-3.5.0.tar.bz2
rm teamspeak3-server_linux_amd64-3.5.0.tar.bz2
cd teamspeak3-server_linux_amd64
touch .ts3server_license_accepted
```

```bash
sudo nano /etc/init.d/teamspeak
```

```bash
#!/bin/sh
### BEGIN INIT INFO
# Provides:         teamspeak
# Required-Start:   $local_fs $network
# Required-Stop:    $local_fs $network
# Default-Start:    2 3 4 5
# Default-Stop:     0 1 6
# Description:      Teamspeak 3 Server
### END INIT INFO

######################################
# Customize values for your needs: "User"; "DIR"

USER="teamspeak"
DIR="/home/teamspeak/teamspeak3-server_linux_amd64"

###### Teamspeak 3 server start/stop script ######

case "$1" in
start)
su $USER -c "${DIR}/ts3server_startscript.sh start"
;;
stop)
su $USER -c "${DIR}/ts3server_startscript.sh stop"
;;
restart)
su $USER -c "${DIR}/ts3server_startscript.sh restart"
;;
status)
su $USER -c "${DIR}/ts3server_startscript.sh status"
;;
*)
echo "Usage: {start|stop|restart|status}" >&2
exit 1
;;
esac
exit 0
```

```bash
sudo chmod +x /etc/init.d/teamspeak
sudo update-rc.d teamspeak defaults

sudo service teamspeak start
```

```bash
sudo ufw allow 9987/udp
sudo ufw allow 30033/tcp
sudo ufw allow 10011/tcp
```

## Midnight Commander
```bash
sudo apt install mc
```

# www-data
```bash
sudo usermod -g www-data spout
sudo chown www-data:www-data /var/www
sudo chmod g+w /var/www
```

## HTTPS / Let's encrypt
https://memo-linux.com/configurer-le-serveur-web-nginx-en-https-avec-letsencrypt-sous-debian-jessie/
https://certbot.eff.org/lets-encrypt/debianstretch-other

```bash
sudo ufw allow https

sudo nano /etc/apt/sources.list

deb http://ftp.debian.org/debian stretch-backports main

sudo apt update
sudo apt install certbot -t stretch-backports

nano /etc/nginx/sites-available/example.com

location ~ /.well-known {
    allow all;
    root /var/www;
}

sudo nginx -t
sudo service nginx reload

sudo certbot certonly --webroot -w /var/www/ -d example.com -d www.example.com --rsa-key-size 4096
sudo certbot renew --dry-run

sudo crontab -e
0 */12 * * * certbot renew --quiet

openssl dhparam -out /etc/ssl/private/dhparams.pem 4096

sudo nano /etc/nginx/nginx.conf

##
# SSL Settings
##

#ssl_protocols TLSv1 TLSv1.1 TLSv1.2; # Dropping SSLv3, ref: POODLE
#ssl_prefer_server_ciphers on;

ssl_ciphers "EECDH+AESGCM:EDH+AESGCM:AES256+EECDH:AES256+EDH";
ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
ssl_prefer_server_ciphers on;
ssl_session_cache shared:SSL:10m;
ssl_dhparam /etc/ssl/private/dhparams.pem;
add_header Strict-Transport-Security "max-age=31536000; includeSubdomains; preload";
```

## Supervisor
```bash
sudo apt install supervisor
```

## Python libs

## Dev
```bash
sudo apt install python-dev
sudo apt install python3-dev
```

### MySQL
```bash
sudo apt install default-libmysqlclient-dev
```

### Pillow (jpeg, tiff, ...):
```bash
sudo apt install libtiff5-dev libjpeg62-turbo-dev zlib1g-dev libfreetype6-dev liblcms2-dev libwebp-dev tcl8.5-dev tk8.5-dev
```

### CURL
```bash
sudo apt install libcurl4-openssl-dev
```

### lxml
```bash
sudo apt install libxml2-dev libxslt1-dev
```

### Cryptography
```bash
sudo apt install libffi-dev
```
