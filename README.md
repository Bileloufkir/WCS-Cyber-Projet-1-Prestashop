# WCS-Cyber-Projet-1-Prestashop

The aim of this project is to deploy an e-commerce site under the [CMS](https://en.wikipedia.org/wiki/Content_management_system) [Prestashop](https://prestashop.fr/), on a remote Linux server, in a secure way.
For installation instructions, go to the INSTALL.md file

## Table of contents
### Contributors
### Linux Setup
### Installing Prestashop
### Troubleshooting
### Improvements

## Contributors
Bilel, Katia, Kelyan, Zhiying

### Roles
Depending on the week, the following roles were divided into the group: Product Owner, Lead Tech, Devops.

We worked with Trello to divide the tasks that the PO identified for the week, and each week we made at least 2 meetings to follow what each member did, according to the Lead Tech instructions.

#### Week 1:
PO: Katia,
Lead Tech: Kelyan
Devops: Bilel, Zhiying

#### Week 2:
PO: Kelyan,
Lead Tech: Katia
Devops: Bilel, Zhiying

#### Week 3:
PO: Bilel,
Lead Tech: Zhiying
Devops: Katia, Kelyan

## Server Setup
### 1. Securing the Linux server
- Linux kernel and package updates
- Create non-root user with sudo access and disable root SSH login
- Use strong passwords (>8 characters, mixture of letters, numbers, special characters)
- Antivirus setup
- Automatic updates

### 2. Securing connections
- Change the SSH port to a port >2034
- Install fail2ban
- Firewall install and setup

## Installing Prestashop
We followed the [Prestashop official recommended pre-requisites](https://devdocs.prestashop-project.org/8/basics/installation/system-requirements/)
### Pre-requisites
- [Apache](https://doc.ubuntu-fr.org/apache2#installation) Server version: Apache/2.4.52 (Ubuntu)
- [PHP](https://doc.ubuntu-fr.org/php#installation) Version 8.1.2-1
- [MySQL](https://doc.ubuntu-fr.org/mysql#installation) Version 8.0.33-0

See INSTALL.md for more.

## Troubleshooting
- Server memory size (Prestashop need at least 2GB RAM for the install)
- At each install attempt, you need to delete and recreate the MySQL databases, otherwise Prestashop will prompt an error during the installation


## Improvements
- Configure Headers Apache
- Change content on /robots.txt to "Disallow /" to avoid indexation (as this is for a test server and not a real merchandising website)
- Launch automatic antivirus scans

#### What we did not to during this project but would be necessary in a larger project:
- MFA configuration
- Server backups
- Encryption: files and passwords (supported by most Linux distributions. With LUKS for example.)
