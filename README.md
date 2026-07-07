# 🛡️ **Backup_Borg** - Borg + Docker + SMS (Free Mobile)

[![Bash - v5+](https://img.shields.io/badge/Bash-5%2B-informational?style=for-the-badge)](https://www.gnu.org/software/bash/) [![License: MIT](https://img.shields.io/badge/License-MIT-green?style=for-the-badge)](LICENSE) 
[![Last Commit](https://img.shields.io/github/last-commit/Axolotty/backup_borg.svg?style=for-the-badge)](https://github.com/Axolotty/backup_borg/commits/main) 

---

## Table des matières

### 🇫🇷 Français
- [Fonctionnalités](#fonctionnalités)
- [Prérequis](#prérequis)
- [Installation rapide](#installation-rapide)
- [Configuration](#configuration)
- [Cron exemple](#cron-exemple)
- [Logs & Monitoring](#logs--monitoring)
- [Notes de sécurité (important)](#notes-de-sécurité-important)
- [FAQ](#faq)
- [Contribuer](#contribuer)
- [Licence](#licence)

### Fonctionnalités
- 🚀 Monte un NAS CIFS et effectue les sauvegardes automatiquement  
- 🐳 Export optionnel des conteneurs Docker en archives `tar.gz`  
- 💾 Sauvegarde complète du serveur avec **BorgBackup** (déduplication + compression)  
- 🧹 Suppression automatique des anciennes sauvegardes (Docker >10j, Borg >40j)  
- 📲 Notifications SMS via Free Mobile pour suivre succès/erreurs  
- 🔁 Paramètres sûrs (`set -euo pipefail`) et exclusions intelligentes

### Prérequis
- Debian/Ubuntu (testé)  
- Droits `sudo`  
- Accès réseau au NAS  
- (Optionnel) Docker pour export des conteneurs  
- (Optionnel) Compte Free Mobile (user + token) pour SMS

### Installation rapide
1. Copier le script et rendre exécutable :
```bash
sudo cp backup /usr/local/bin/backup
sudo chmod +x /usr/local/bin/backup
```
2. Créer le dossier de configuration et les fichiers secrets :
```bash
sudo mkdir -p /etc/backup_script
sudo nano /etc/backup_script/smb.conf   # Identifiants CIFS
sudo nano /etc/backup_script/borg_pass  # Mot de passe Borg, chmod 600
sudo chmod 600 /etc/backup_script/*
```
3. Tester le script :
```bash
sudo /usr/local/bin/backup -h    # aide
sudo /usr/local/bin/backup      # sauvegarde manuelle
```

### Configuration
Modifier les variables en début de script :

```bash
SOURCE_DIR="/"
DEST_DIR="/var/backup"
MOUNT_POINT="/var/backup"
NAS_SERVER="192.168.1.10"
CONFIG_FILE="/etc/backup_script/smb.conf"
BORG_PASSPHRASE="$(cat /etc/backup_script/borg_pass)"
DOCKER="1"
BACKUP_DOCKER="/var/backups/docker/"
NAME_FOR_NOTIFY="mon-serveur"
FREE_MOBILE_USER="0123456789"
FREE_MOBILE_CODE_AUTH="abcdEFGhijk"
PHONE_NUMBER="+33XXXXXXXXX"
```

CIFS `/etc/backup_script/smb.conf` :  
```
username=MON_USER
password=MON_MOT_DE_PASSE
domain=MON_DOMAINE  # optionnel
```
> 🔒 Protéger le fichier : `chmod 600 /etc/backup_script/smb.conf`

### Cron exemple
Exécution chaque nuit à 03h00 :  
```
0 3 * * * /usr/local/bin/backup >> /var/log/backup_cron.log 2>&1
```

### Logs & Monitoring
- Fichier log principal : `/var/log/backup.log`  
- Suivi des exports Docker, sauvegarde Borg, montage/démontage NAS  
- Intégration possible avec Prometheus/ELK/syslog

### Notes de sécurité (important)
- Ne jamais rendre `BORG_PASSPHRASE` world-readable  
- Préférer fichier ou variable d'environnement pour secrets  
- Ajouter un lockfile pour éviter les exécutions concurrentes  
- Tester régulièrement la restauration

### FAQ
**Q - Pourquoi Borg ?**  
A - Déduplication, compression et sécurité intégrée pour vos backups.  

**Q - Les SMS ne fonctionnent pas ?**  
A - Tester l’API Free Mobile manuellement :  
```bash
curl -G --data-urlencode "msg=hello" "https://smsapi.free-mobile.fr/sendmsg?user=USER&pass=CODE"
```

**Q - Les exports Docker sont volumineux ?**  
A - Oui. Préférer `docker save` ou un registre externe si nécessaire.

### Contribuer
- Ajouter un service systemd + timer pour planification propre  
- Ajouter un lockfile  
- Ajouter mode dry-run / tests unitaires  
- Support pour notifications Slack/Teams

### Licence
MIT © Axolotty

---

## 🇬🇧 English
- [Features](#features)
- [Prerequisites](#prerequisites)
- [Quick start](#quick-start)
- [Configuration](#configuration-1)
- [Cron example](#cron-example)
- [Logs & Monitoring](#logs--monitoring-1)
- [Security notes (must-read)](#security-notes-must-read)
- [FAQ](#faq-1)
- [Contributing](#contributing)
- [License](#license)

### Features
- 🚀 Mount a CIFS NAS and run backups automatically  
- 🐳 Optional export of Docker containers to `tar.gz` archives  
- 💾 Full server backup with **BorgBackup** (deduplication + compression)  
- 🧹 Auto-prune old backups (Docker >10d, Borg >40d)  
- 📲 SMS notifications via Free Mobile for success/error tracking  
- 🔁 Safe defaults (`set -euo pipefail`) and smart excludes

### Prerequisites
- Debian/Ubuntu (tested)  
- `sudo` privileges  
- Network access to NAS  
- (Optional) Docker access  
- (Optional) Free Mobile account (user + auth token) for SMS notifications

### Quick start
1. Copy script and make executable:
```bash
sudo cp backup /usr/local/bin/backup
sudo chmod +x /usr/local/bin/backup
```
2. Create config folder and credentials:
```bash
sudo mkdir -p /etc/backup_script
sudo nano /etc/backup_script/smb.conf
sudo nano /etc/backup_script/borg_pass
sudo chmod 600 /etc/backup_script/*
```
3. Test manually:
```bash
sudo /usr/local/bin/backup -h
sudo /usr/local/bin/backup
```

### Configuration
Set variables at the top of the script:

```bash
SOURCE_DIR="/"
DEST_DIR="/var/backup"
MOUNT_POINT="/var/backup"
NAS_SERVER="192.168.1.10"
CONFIG_FILE="/etc/backup_script/smb.conf"
BORG_PASSPHRASE="$(cat /etc/backup_script/borg_pass)"
DOCKER="1"
BACKUP_DOCKER="/var/backups/docker/"
NAME_FOR_NOTIFY="my-server"
FREE_MOBILE_USER="0123456789"
FREE_MOBILE_CODE_AUTH="abcdEFGhijk"
PHONE_NUMBER="+33XXXXXXXXX"
```

CIFS file `/etc/backup_script/smb.conf`:  
```
username=MY_USER
password=MY_PASSWORD
domain=MY_DOMAIN  # optional
```
> 🔒 Protect credentials: `chmod 600 /etc/backup_script/smb.conf`

### Cron example
Nightly at 03:00:  
```
0 3 * * * /usr/local/bin/backup >> /var/log/backup_cron.log 2>&1
```

### Logs & Monitoring
- Main log: `/var/log/backup.log`  
- Tracks Docker export, Borg backup, NAS mount/unmount  
- Can be integrated into Prometheus/ELK/syslog

### Security notes (must-read)
- Never make `BORG_PASSPHRASE` world-readable  
- Prefer environment variable or file for secrets  
- Add lockfile to prevent concurrent runs  
- Test restores regularly

### FAQ
**Q - Why Borg?**  
A - Deduplication, compression and built-in encryption for backups.  

**Q - SMS not received?**  
A - Test Free Mobile API manually:  
```bash
curl -G --data-urlencode "msg=hello" "https://smsapi.free-mobile.fr/sendmsg?user=USER&pass=CODE"
```

**Q - Docker exports are large?**  
A - Consider `docker save` or external registry; export containers only if necessary.

### Contributing
- Add systemd service + timer for clean scheduling  
- Add lockfile  
- Add dry-run / unit tests  
- Support for Slack/Teams notifications

### License
MIT © Axolotty
