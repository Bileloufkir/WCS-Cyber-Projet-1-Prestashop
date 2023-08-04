# WCS-Cyber-Projet-1-Prestashop

The aim of this project is to deploy an e-commerce site under Prestashop, on a remote Linux server, in a secure way.

## Table of contents
#### Pre-requisites with versions
#### Server setup
#### Installation
#### Troubleshooting

## Pre-requisites
- [Apache](https://doc.ubuntu-fr.org/apache2#installation) Server version: Apache/2.4.52 (Ubuntu)
- [PHP](https://doc.ubuntu-fr.org/php#installation) Version 8.1.2-1
- [MySQL](https://doc.ubuntu-fr.org/mysql#installation) Version 8.0.33-0

You can install these packages with the command line
```bash
apt-get install apache2 libapache2-mod-php mysql-server
apt-get install php7.0-cli php7.0-common php7.0-curl php7.0-zip php7.0-gd php7.0-mysql php7.0-xml php7.0-mcrypt php7.0-mbstring php7.0-json php7.0-intl
```

## Server Setup
#### Create non root users with sudo access:
```bash
sudo useradd -m -s /bin/bash -g <groupeName> -G sudo <userName>
```

### 1. Securing the Linux server
#### Linux kernel and package updates:
```bash
sudo apt-get update && upgrade
```

#### Disable root login on SSH:

In the ```/etc/ssh/sshd_config file```, edit PermitRootLogin to no:
```bash
PermitRootLogin no
```

#### Antivirus setup:
```bash
sudo apt install clamav
sudo freshclam #updates
clamscan -r / #scan
```

#### Automatic updates:

Install the "unattended-upgrade": 
```bash
sudo apt install unattended upgrade
```
Edit the following  file: ```/etc/apt/apt.conf.d/50unattended-upgradeset``` and uncomment (remove the "//") ```Unattended-Upgrade::Automatic-Reboot 'true';```

Save file

### 2. Securing connections
#### Change the SSH port to a port >2034, for example port 2050.
In file /etc/ssh/sshd_config:
```bash
Include /etc/ssh/sshd_config.d/*.conf
Port 2050
```

Reboot the service
```bash
sudo systemctl restart sshd
```

#### Verification that the service listens to the 2050 port:
```bash
netstat -tuln
```

#### Use fail2ban/denyhost as IDS

Install fail2ban:
```bash
sudo apt install fail2ban
```
Config the following file: ```sudo nano /etc/fail2ban/jail.d/ssh.conf```
with this:
```bash
[sshd]
enabled = true
port = 2050
logpath = /var/log/auth.log
maxretry = 2
bantime = 3600 # 1hour ban
```
Restart fail2ban:
```bash
sudo systemctl start fail2ban
```

To launch fail2ban on restart, execute
```bash
sudo systemctl enable fail2ban
```
Fail2ban is now installed and setup.

To check the connections, the following commandline ```sudo fail2ban-client status sshd``` will show:
```bash
Status for the jail: sshd
|- Filter
|  |- Currently failed: 1
|  |- Total failed:     6
|  `- File list:        /var/log/auth.log
`- Actions
   |- Currently banned: 0
   |- Total banned:     2
   `- Banned IP list:
```

#### Firewall install and setup:

Install the tools to generate the SSL certificate:
```bash
sudo apt install certbot python3-certbot-apache
```

Create the Prestashop configuration in ```/etc/apache2/sites-available/prestashop.conf```:
```bash
<VirtualHost *:80>
ServerAdmin [your email address]
ServerName [your domain name] #as for us groupe1.dev-cyber.wilders.dev

DocumentRoot /var/www/prestashop

<Directory /var/www/prestashop>
Options +FollowSymlinks
AllowOverride All
Require all granted
</Directory>

ErrorLog /var/log/apache2/prestashop-error_log
CustomLog /var/log/apache2/prestashop-access_log common
RewriteEngine on
RewriteCond %{SERVER_NAME}=[your domain name] #as for us groupe1.dev-cyber.wilders.dev
RewriteRule ^ https://%{SERVER_NAME}%{REQUEST_URI} [END,NE,R=permanent]
</VirtualHost>
```

and delete the other default-config file.

Restart apache2:
```bash
sudo systemctl reload apache2
```

Check Firewall status:
```bash
sudo ufw status
```

Check SSH version:
```bash
ssh -V
sudo nano /etc/default/ufw
sudo ufw allow ssh
cat /etc/ssh/sshd_config
```

Enable firewall:
```bash
sudo ufw enable
```

Manage firewall rules:
```bash
sudo ufw allow 'openSSH'
sudo ufw allow 'Apache Full'
sudo ufw delete allow 'Apache'
```

Request the generation of the ssl certificate:
```bash
sudo certbot --apache
```

#### Disable all from /var/www/prestashop/robots.txt
```bash
sudo nano /var/www/prestashop/robots.txt
```
Delete all rules and leave only "Disable /"

### 3. IAM
- use a user account and do not remain logged in as root
- use strong passwords (+ 8 characters, mixture of letters, numbers, special characters)
- avoid directory listing by adding ```"-Indexes"``` in the file ```/var/www/prestashop/.htaccess```:
```bash
"Options +FollowSymlinks -Indexes"
```

## Installing Prestashop
#### Download the [PrestaShop](https://prestashop.fr/prestashop-edition-basic/) install from the official website (you have to create an account):
```bash
sudo wget <file_link>.zip
```

### 1. Database creation
For the installation, we first need to create a database with MySQL, and a user with all privileges to create the tables needed by Prestashop:
```sql
CREATE DATABASE prestashop_db;
CREATE USER 'new_user'@'localhost' IDENTIFIED BY 'passwd';
GRANT ALL PRIVILEGES ON prestashop_db.* TO 'new_user'@'localhost';
FLUSH PRIVILEGES
```

### 2. Follow Prestashop directions with your domain name and SQL database/user names
(add screenshots and steps)
### 3. At the end of the installation, delete the /install file for security issues.

## Troubleshooting
- Memory size (Prestashop need at least 2GB RAM for the install)
- At each install attempt, you need to delete and recreate the MySQL databases, otherwise Prestashop will prompt an error during the installation
