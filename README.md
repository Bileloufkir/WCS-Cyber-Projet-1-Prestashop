# WCS-Cyber-Projet-1-Prestashop

L'objectif de ce projet est de déployer un site e-commerce sous Prestashop, sur un serveur Linux à distance, le tout de manière sécurisée.

## Sommaire

- Installation
- Configuration
- Troubleshooting

## Configuration du serveur

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

Vérifier le statut du Firewall :
```sudo ufw status```

Vérifier la version SSH :
```ssh -V```
```sudo nano /etc/default/ufw```
```sudo ufw allow ssh```
```sudo ufw allow 22```
```cat /etc/ssh/sshd_config```

Activer firewall :
```sudo ufw enable```

Gérer les règles du firewall :
```sudo ufw allow 'openSSH'```
```sudo ufw allow 'Apache Full'```
```sudo ufw delete allow 'Apache'```

Demander la génération du certificat ssl :
```sudo certbot --apache```

### 3. IAM
- utiliser chacun un compte user et ne pas rester loggé en root
- utiliser des mdp forts (+ de 8 caractères, mélange de lettres, chiffres, caractères spéciaux)
- Make Sure No Non-Root Accounts Have UID Set To 0

## Installation de Prestashop
- Télécharger l'install [PrestaShop](https://prestashop.fr/prestashop-edition-basic/) sur le site officiel (se créer un compte) : ```sudo wget liendufichier.zip```
- [Apache](https://doc.ubuntu-fr.org/apache2#installation)
- [PHP](https://doc.ubuntu-fr.org/php#installation)
- [MySQL](https://doc.ubuntu-fr.org/mysql#installation)

## Troubleshooting

## Idéalement
- Logging and Auditing
- System Accounting with auditd
- Make backups
