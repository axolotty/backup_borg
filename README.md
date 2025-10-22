# 🛡️ Backup Script - Borg + Docker + SMS Notifications

[![Bash Version](https://img.shields.io/badge/bash-5.1-blue.svg)](https://www.gnu.org/software/bash/)
[![License](https://img.shields.io/badge/license-MIT-green.svg)](LICENSE)
[![Last Commit](https://img.shields.io/github/last-commit/Axolotty/repo.svg)](https://github.com/Axolotty/backup_borg/commits/main)
---

## Table des matières
1. [Description](#description)
2. [Prérequis / Requirements](#prérequis--requirements)
3. [Variables à configurer / Configurable Variables](#variables-à-configurer--configurable-variables)
4. [Installation & Utilisation / Installation & Usage](#installation--utilisation--installation--usage)
5. [Cron recommandé / Recommended Cron](#cron-recommandé--recommended-cron)
6. [Logs](#logs)
7. [Notifications SMS / SMS Notifications](#notifications-sms--sms-notifications)
8. [Sécurité / Security](#sécurité--security)
9. [Licence / License](#licence--license)

---

## Description
**FR 🇫🇷:**
Ce script Bash permet de réaliser des sauvegardes complètes d'un serveur Linux avec **BorgBackup**, d'exporter les **conteneurs Docker** si nécessaire, de monter un NAS CIFS et d'envoyer des notifications SMS via Free Mobile.

**EN 🇬🇧:**
This Bash script enables full backups of a Linux server using **BorgBackup**, optional **Docker container exports**, CIFS NAS mounting, and SMS notifications via Free Mobile.

**Fonctionnalités / Features:**
- 🚀 Montage automatique d’un NAS CIFS / Automatic CIFS NAS mounting
- 📦 Vérification et installation de BorgBackup et cifs-utils / Checks and installs BorgBackup and cifs-utils
- 🐳 Sauvegarde Docker optionnelle / Optional Docker container backup
- 💾 Sauvegarde complète du système / Full system backup with BorgBackup
- 🧹 Nettoyage automatique des anciennes sauvegardes / Auto-clean old Docker (>10 days) & Borg (>40 days) backups
- 🔒 Démontage du NAS après sauvegarde / NAS unmount after backup
- 📲 Notifications SMS sur l’état de la sauvegarde / SMS notifications on backup status

---

## Prérequis / Requirements
- Debian/Ubuntu
- `sudo` pour installer les paquets et monter le NAS
- `borgbackup` (installation automatique possible via le script)
- `cifs-utils` pour le montage CIFS
- Docker (si option `DOCKER=1`) et accès au daemon
- NAS accessible sur le réseau
- Compte Free Mobile pour notifications SMS (optionnel)

---

## Variables à configurer / Configurable Variables
```bash
SOURCE_DIR="/"
DEST_DIR="/var/backup"
MOUNT_POINT="/var/backup"
NAS_SERVER=""
CONFIG_FILE="/etc/backup_script/smb.conf"
BORG_PASSPHRASE="$(cat /etc/backup_script/borg_pass)"
DOCKER="1"
BACKUP_DOCKER="/var/backups/docker/"
NAME_FOR_NOTIFY=""
FREE_MOBILE_USER=""
FREE_MOBILE_CODE_AUTH=""
PHONE_NUMBER=""
```

**Exemple de fichier CIFS / Example CIFS file:**
```
username=MON_USER
password=MON_MOT_DE_PASSE
domain=MON_DOMAINE
```
> ⚠️ Protégez ce fichier (`chmod 600`).

---

## Installation & Utilisation / Installation & Usage
```bash
sudo cp backup /usr/local/bin/backup
sudo chmod +x /usr/local/bin/backup

# Lancer la sauvegarde / Run backup
sudo /usr/local/bin/backup

# Afficher l'aide / Show help
sudo /usr/local/bin/backup -h
```

---

## Cron recommandé / Recommended Cron
Sauvegarde quotidienne à 3h / Daily backup at 3AM:
```
0 3 * * * /usr/local/bin/backup >> /var/log/backup_cron.log 2>&1
```

---

## Logs
Toutes les opérations sont enregistrées dans `/var/log/backup.log`.
- Horodatage précis de chaque action / Precise timestamp for each operation
- Suivi des erreurs / Error tracking

---

## Notifications SMS / SMS Notifications
Le script utilise l'API Free Mobile :
```
https://smsapi.free-mobile.fr/sendmsg?user=USER&pass=CODE&msg=...
```
- Messages automatiquement encodés / Messages automatically encoded
- Suivi en temps réel de l’état des sauvegardes / Real-time backup status updates

---

## Sécurité / Security
- 🔑 `BORG_PASSPHRASE` dans un fichier sécurisé (`/etc/backup_script/borg_pass`)
- 🔒 Vérifier l’accès au NAS et à Docker avant d’exécuter
- ⚠️ Éviter d’exécuter plusieurs instances simultanément (utiliser un lock si nécessaire)
- 🔍 Tester l’API Free Mobile séparément

---

## Licence / License
MIT License

---

> 🎉 Un script élégant, complet et automatisé pour protéger vos données avec style, notifications et Docker intégrés.

