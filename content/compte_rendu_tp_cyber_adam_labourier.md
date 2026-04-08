# Compte Rendu — TP Cybersécurité
**Adam Labourier** | Ynov Lyon | Mars 2026

---

## Mission 01 — Déploiement et gestion des utilisateurs

Création de quatre comptes avec rôles distincts :

```bash
sudo adduser admin_sys
sudo adduser monitoring
sudo adduser deploiement
sudo adduser backup_service
```

**Configuration PAM (pam_pwquality)**
Installation du module et configuration dans `/etc/security/pwquality.conf` :
- Longueur minimale : 14 caractères
- Exigences : majuscule + chiffre + caractère spécial

```bash
sudo apt install libpam-pwquality
sudo nano /etc/security/pwquality.conf
```

**Politique d'expiration des mots de passe** via `/etc/login.defs` :
- Expiration max : 90 jours
- Préavis : 7 jours
- Délai de grâce : 3 jours

**Compte backup_service** — shell non interactif :

```bash
sudo usermod -s /usr/sbin/nologin backup_service
```

---

## Mission 02 — Configuration des permissions et répertoires

Création et configuration des répertoires avec les bons propriétaires et modes :

| Répertoire | Propriétaire | Mode |
|---|---|---|
| `/srv/application/` | deploiement:www-data | 750 |
| `/srv/logs/` | monitoring:adm | 2750 (SGID) |
| `/var/backups/app/` | backup_service:backup | 700 |

Configuration des ACL pour permettre à `monitoring` de lire `/srv/logs/` sans en être propriétaire.

**Immutabilité des fichiers critiques** :

```bash
sudo chattr +i /etc/ssh/sshd_config
```

---

## Mission 03 — Sécurisation SSH et accès distant

**Installation du serveur SSH**

```bash
sudo apt install openssh-server
```

**Génération des paires de clés Ed25519** pour chaque compte requérant l'accès SSH :

```bash
sudo -u admin_sys    ssh-keygen -t ed25519 -C "admin_sys@server"    -f /home/admin_sys/.ssh/id_ed25519
sudo -u deploiement  ssh-keygen -t ed25519 -C "deploiement@server"  -f /home/deploiement/.ssh/id_ed25519
sudo -u monitoring   ssh-keygen -t ed25519 -C "monitoring@server"   -f /home/monitoring/.ssh/id_ed25519
```

**Durcissement de sshd_config** — modification des paramètres clés :
- Port non standard
- `PermitRootLogin no`
- `PasswordAuthentication no`
- `AuthorizedKeysFile .ssh/authorized_keys`

Le fichier a été rendu immuable après modification :

```bash
sudo chattr -i /etc/ssh/sshd_config   # retrait temporaire pour édition
sudo nano /etc/ssh/sshd_config
sudo chattr +i /etc/ssh/sshd_config   # remise en place du verrou
```

**Configuration de fail2ban** — protection contre les attaques par force brute :

```bash
sudo apt install fail2ban -y
sudo nano /etc/fail2ban/jail.local
```

Paramètres appliqués dans `jail.local` :
- Tentatives max : 3
- Durée de bannissement : 1 heure (3600s)

Vérification :

```bash
sudo fail2ban-client status sshd
```

---

*Document généré le 13 mars 2026*
