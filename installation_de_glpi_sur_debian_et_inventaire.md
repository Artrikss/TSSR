# Installation de GLPI sur Debian et Remontée d'Inventaire

Guide complet pas à pas pour déployer un serveur **GLPI** sous **Debian** (avec **Apache2**, **MariaDB** et **PHP**), configurer un accès direct via l'adresse IP (sans le suffixe `/glpi`) et automatiser la remontée d'inventaire matériel et logiciel depuis des postes **Windows** et **Debian** via **GLPI Agent**.

---

## 📋 Sommaire

1. [Installation des prérequis (Apache2, MariaDB, PHP)](#1-installation-des-prérequis-apache2-mariadb-php)
2. [Création de la base de données MariaDB](#2-création-de-la-base-de-données-mariadb)
3. [Téléchargement et déploiement des fichiers GLPI](#3-téléchargement-et-déploiement-des-fichiers-glpi)
4. [Configuration d'Apache2 (accès direct sans `/glpi`)](#4-configuration-dapache2-accès-direct-sans-glpi)
5. [Finalisation de l'installation web et sécurisation](#5-finalisation-de-linstallation-web-et-sécurisation)
6. [Remontée d'inventaire via GLPI Agent](#6-remontée-dinventaire-via-glpi-agent)
   - [A. Depuis un poste client Windows](#a-depuis-un-poste-client-windows)
   - [B. Depuis un serveur ou client Debian](#b-depuis-un-serveur-ou-client-debian)

---

## 1. Installation des prérequis (Apache2, MariaDB, PHP)

Mets à jour les dépôts du système et installe le serveur web, le moteur de base de données ainsi que toutes les extensions PHP exigées par GLPI (notamment `bcmath` pour les QR codes et `ldap` pour Active Directory) :

```bash
sudo apt update && sudo apt upgrade -y
sudo apt install -y apache2 mariadb-server wget curl perl \
php php-mysql php-curl php-gd php-intl php-mbstring php-xml \
php-zip php-bz2 php-ldap php-apcu php-bcmath
```

> **Vérification :** Assure-toi que les services sont actifs avec `sudo systemctl status apache2 mariadb --no-pager`.

---

## 2. Création de la base de données MariaDB

Connecte-toi à l'invite SQL en tant que `root` :

```bash
sudo mysql -u root
```

Exécute les requêtes SQL suivantes pour créer la base `glpidb` en UTF-8 complet (`utf8mb4`) ainsi que l'utilisateur SQL dédié (remplace `MotDePasseRobuste` par ton propre mot de passe) :

```sql
CREATE DATABASE glpidb CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
CREATE USER 'glpiuser'@'localhost' IDENTIFIED BY 'MotDePasseRobuste';
GRANT ALL PRIVILEGES ON glpidb.* TO 'glpiuser'@'localhost';
FLUSH PRIVILEGES;
EXIT;
```

> **Vérification :** Teste l'accès avec `mysql -u glpiuser -p`, saisis le mot de passe défini, puis quitte avec `EXIT;`.

---

## 3. Téléchargement et déploiement des fichiers GLPI

Récupère automatiquement la dernière archive `.tgz` officielle depuis GitHub dans `/tmp`, extrais-la dans `/var/www/html/` et attribue les droits au compte utilisateur d'Apache (`www-data`) :

```bash
cd /tmp
curl -s https://api.github.com/repos/glpi-project/glpi/releases/latest | grep "browser_download_url.*tgz" | cut -d '"' -f 4 | wget -i - -O glpi.tgz
sudo tar -xzvf /tmp/glpi.tgz -C /var/www/html/
sudo chown -R www-data:www-data /var/www/html/glpi
sudo chmod -R 755 /var/www/html/glpi
```

> **Vérification :** Liste le contenu du répertoire avec `ls -la /var/www/html/glpi` (les dossiers `public`, `files` et `config` doivent être présents et appartenir à `www-data`).

---

## 4. Configuration d'Apache2 (accès direct sans `/glpi`)

### Sécuriser les cookies de session PHP

Active l'option `session.cookie_httponly` dans la configuration PHP d'Apache pour valider les contrôles de sécurité de GLPI :

```bash
PHP_VER=$(php -r 'echo PHP_MAJOR_VERSION.".".PHP_MINOR_VERSION;')
sudo sed -i 's/^session.cookie_httponly =.*/session.cookie_httponly = On/' /etc/php/${PHP_VER}/apache2/php.ini
```

### Créer le VirtualHost dédié

Crée le fichier de configuration `/etc/apache2/sites-available/glpi.conf` en pointant le `DocumentRoot` directement sur `/var/www/html/glpi/public` :

```bash
sudo bash -c 'cat << EOF > /etc/apache2/sites-available/glpi.conf
<VirtualHost *:80>
    DocumentRoot /var/www/html/glpi/public

    <Directory /var/www/html/glpi/public>
        Require all granted
        RewriteEngine On
        RewriteCond %{REQUEST_FILENAME} !-f
        RewriteRule ^(.*)$ index.php [QSA,L]
    </Directory>
</VirtualHost>
EOF'
```

### Activer le site et redémarrer Apache

Active le module `rewrite`, désactive le site par défaut d'Apache (`000-default.conf`), active le site GLPI et redémarre le service :

```bash
sudo a2enmod rewrite
sudo a2dissite 000-default.conf
sudo a2ensite glpi.conf
sudo systemctl restart apache2
```

> **Vérification :** Ouvre un navigateur et accède à `http://IP_DU_SERVEUR` (sans `/glpi`). L'assistant d'installation web de GLPI s'affiche directement.

---

## 5. Finalisation de l'installation web et sécurisation

1. **Assistant d'installation web :**
   - **Langue & Licence :** Choisis le français, accepte la licence et clique sur **Installer**.
   - **Prérequis :** Vérifie que tous les contrôles sont au vert.
   - **Connexion SQL :**
     - Serveur SQL : `localhost`
     - Utilisateur SQL : `glpiuser`
     - Mot de passe SQL : `MotDePasseRobuste`
   - **Base de données :** Sélectionne la base existante **`glpidb`**.
   - **Authentification initiale :** Connecte-toi avec les identifiants par défaut **`glpi` / `glpi`**.

2. **Nettoyage et sécurisation post-installation :**
   Supprime immédiatement le script d'installation et l'archive temporaire :

```bash
sudo rm /var/www/html/glpi/install/install.php
rm -f /tmp/glpi.tgz
```

> **Bonne pratique :** Modifie immédiatement les mots de passe des comptes par défaut (`glpi`, `tech`, `normal`, `post-only`) dans **Administration > Utilisateurs**.

---

## 6. Remontée d'inventaire via GLPI Agent

### A. Depuis un poste client Windows

1. Télécharge la dernière version de l'installeur `.msi` depuis le dépôt officiel [glpi-project/glpi-agent](https://github.com/glpi-project/glpi-agent/releases).
2. Exécute le fichier `.msi` **en tant qu'administrateur**.
3. À l'étape **Choose Targets** :
   - Laisse **Local Target** vide.
   - Dans **Remote Targets**, renseigne l'URL racine de ton serveur GLPI : `http://IP_DU_SERVEUR/` *(sans `/glpi`)*.
   - Laisse **Quick installation** coché et termine l'installation.
4. Force un premier inventaire immédiat en ouvrant `http://localhost:62354` dans le navigateur du poste Windows, puis clique sur **Force an Inventory**.

### B. Depuis un serveur ou client Debian

1. Télécharge le script d'installation Perl officiel et lance l'installation en pointant vers le serveur GLPI avec l'option `--runnow` :

```bash
cd /tmp
curl -s https://api.github.com/repos/glpi-project/glpi-agent/releases/latest | grep "browser_download_url.*linux-installer.pl" | cut -d '"' -f 4 | wget -i - -O glpi-agent-installer.pl
sudo perl /tmp/glpi-agent-installer.pl --install --server=http://IP_DU_SERVEUR/ --runnow
```

2. Vérifie que le service tourne et contrôle les logs d'envoi :

```bash
sudo systemctl status glpi-agent --no-pager
sudo journalctl -u glpi-agent -n 20 --no-pager
```

3. Nettoie le fichier d'installation dans `/tmp` :

```bash
rm -f /tmp/glpi-agent-installer.pl
```

> **Vérification finale :** Sur l'interface web de GLPI, rends-toi dans **Parc > Ordinateurs**. Tes machines Windows et Debian apparaissent automatiquement avec leur inventaire complet.