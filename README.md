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
