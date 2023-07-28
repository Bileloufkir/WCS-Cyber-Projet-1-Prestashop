# WCS-Cyber-Projet-1-Prestashop

L'objectif de ce projet est de déployer un site e-commerce sous Prestashop, sur un serveur Linux à distance, le tout de manière sécurisée.


## Table of contents

- Installation
- Configuration
- Troubleshooting

## Configuration

### 1. Sécurisation du serveur Linux
- Mises à jour des paquets et noyau Linux : ```sudo apt-get update && upgrade```
- Installer uniquement les apt nécessaires et désactiver les services Linux dont on ne se sert pas
- Désactiver la connexion root
- Vérifier Linux Kernel /etc/sysctl.conf

### 2. Sécurisation des connexions
- Find Listening Network Ports
- Turn Off IPv6 only if NOT using it on Linux
- Use fail2ban/denyhost as IDS
- Encryption: files and passwords
Full disk encryption is a must for securing data, and is supported by most Linux distributions. See how to encrypting harddisk using LUKS on Linux. Make sure swap is also encrypted. Require a password to edit bootloader.
- Firewall :
Installer les outils pour génerer le certificat SSL :
```sudo apt install certbot python3-certbot-apache```
modifier la configuration Prestashop :
```sudo nano /etc/apache2/sites-available/prestashop.conf```
et vérifier que le Servername est bon :
```ServerName groupe1.dev-cyber.wilders.dev```
Redémarrer apache2 :
```sudo systemctl reload apache2```
verifier Firewall :
```sudo ufw status```
verifier version ssh or ssh exist :
```ssh -V```
```sudo nano /etc/default/ufw```
```sudo ufw allow ssh```
```sudo ufw allow 22```
```cat /etc/ssh/sshd_config```
Activer firewall :
```sudo ufw enable```
add rules pour firewall :
```sudo ufw allow 'openSSH'```
```sudo ufw allow 'Apache Full'```
```sudo ufw delete allow 'Apache'```
demander certbot generer ssl certification :
```sudo certbot --apache```

### 3. IAM
- utiliser chacun un compte user et ne pas rester loggé en root
- utiliser des mdp forts (+ de 8 caractères, mélange de lettres, chiffres, !ù%)
- choix de ne pas de password aging (veille des bonnes pratiques) ?
- Make Sure No Non-Root Accounts Have UID Set To 0

## Installation
- Télécharger l'install [PrestaShop](https://prestashop.fr/prestashop-edition-basic/) sur le site officiel (se créer un compte) : ```sudo wget liendufichier.zip```
- [Apache](https://httpd.apache.org/)
- [PHP](https://www.php.net/manual/fr/intro-whatis.php)
- [MySQL](https://www.mysql.com/fr/)

## Troubleshooting

## Later
- Logging and Auditing
- System Accounting with auditd
- Install And Use Intrusion Detection System
- Make backups
