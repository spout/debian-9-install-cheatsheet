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
