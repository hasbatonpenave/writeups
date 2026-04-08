# Journal d'installation serveur

## 1. Prise en main et sécurisation initiale
```bash
sudo adduser francis
sudo usermod -aG sudo francis
```

---

## 2. Mise à jour et préparation du système
```bash
sudo apt update && sudo apt upgrade
ping 8.8.8.8
sudo ss -tulnp
systemctl status ssh
```

![ping](https://i.imgur.com/aLQDoPT.png)
![ss -tulnp](https://img.lightshot.app/ETLqn5fUQrW1erodWMeBDw.png)

---

## 3. Installation du serveur web Apache
```bash
sudo apt install apache2
sudo systemctl status apache2
sudo systemctl enable apache2   # Démarrage automatique
curl http://localhost            # Vérification qu'Apache est actif
```

![apache2 status](https://i.imgur.com/miMH1HL.png)

### PHP
```bash
sudo apt install libapache2-mod-php php
sudo systemctl restart apache2
```

### Configuration du VirtualHost
```bash
sudo nano /etc/hosts
sudo a2dissite 000-default.conf   # Désactivation du site par défaut
sudo systemctl reload apache2

sudo mkdir -p /var/www/monsiteparfait
sudo chown -R $USER:$USER /var/www/monsiteparfait

nano /var/www/monsiteparfait/index.html
sudo nano /etc/apache2/sites-available/monsiteparfait.conf

sudo a2ensite monsiteparfait.conf
sudo apache2ctl configtest
sudo systemctl reload apache2

echo "<?php phpinfo(); ?>" > /var/www/monsite/info.php
```

![/etc/hosts](https://i.imgur.com/QrihHDP.png)
![index.html](https://i.imgur.com/QyvM7Ys.png)
![monsiteparfait.conf](https://i.imgur.com/uhk4S8i.png)
![configtest](https://i.imgur.com/sjD9RLF.png)

---

## 4. Mise en place du HTTPS
```bash
sudo apt install certbot python3-certbot-apache
sudo certbot --apache -d ynov-cyb-05.secureaks.tech
```

![certbot](https://i.imgur.com/Rsw2q54.png)
![résultat HTTPS](https://i.imgur.com/4KpYVGd.png)

---

## 5. Installation de DokuWiki
```bash
cd /var/www/monsiteparfait

# Téléchargement de la dernière version stable
wget https://download.dokuwiki.org/src/dokuwiki/dokuwiki-stable.tgz

# Extraction dans le dossier courant
tar -xzvf dokuwiki-stable.tgz --strip-components=1

# Nettoyage
rm dokuwiki-stable.tgz

# Permissions
chown -R www-data:www-data /var/www/monsiteparfait
```

Accès à l'installeur : https://ynov-cyb-05.secureaks.tech/install.php

### ⚠️ Erreur rencontrée

![Erreur](https://i.imgur.com/m7AYC97.png)

**Résolution — module PHP manquant :**
```bash
sudo apt update
sudo apt install php-xml
sudo systemctl restart apache2
```


configuration via install.php https://i.imgur.com/MNooyqF.png
https://i.imgur.com/ENuzhBw.png

on supprime le /install.php par securtié via rm var/www/monsiteparfait/install.php

![[Pasted image 20260223094337.png]]