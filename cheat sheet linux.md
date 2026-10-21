# Cheat Sheet Linux 

## 1. Navigation & Manipulation de Fichiers
| Commande | Description / Action |
| :--- | :--- |
| `pwd` | Affiche le dossier courant (Print Working Directory) |
| `ls -la` | Liste tous les fichiers/dossiers (y compris cachés) avec détails |
| `cd /chemin/vers/dossier` | Change de répertoire |
| `cd ~` ou `cd` | Retourne dans le répertoire personnel (home) |
| `cd -` | Retourne au répertoire précédent |
| `mkdir -p dossier/sous_dossier` | Crée un dossier (et ses parents si besoin) |
| `cp -r source/ destination/` | Copie un dossier et son contenu récursivement |
| `mv source destination` | Déplace ou nomme/renomme un fichier ou dossier |
| `rm -rf dossier/` | Supprime un dossier et son contenu (attention : définitif) |
| `touch fichier.txt` | Crée un fichier vide ou met à jour la date d'accès |

---

## 2. Lecture, Recherche & Édition de Fichiers
| Commande | Description / Action |
| :--- | :--- |
| `cat fichier` | Affiche tout le contenu d'un fichier |
| `less fichier` | Affiche le fichier avec défilement (`q` pour quitter) |
| `head -n 20 fichier` | Affiche les 20 premières lignes d'un fichier |
| `tail -f /var/log/syslog` | Suit en direct l'évolution d'un fichier de log |
| `grep -ir "mot" /dossier/` | Recherche "mot" de façon récursive sans tenir compte de la casse |
| `find /var/log -name "*.log"` | Recherche tous les fichiers `.log` dans `/var/log` |
| `nano fichier` | Éditeur de texte simple en ligne de commande |
| `vim fichier` | Éditeur avancé (`i` pour insérer, `:wq` pour sauvegarder et quitter) |

---

## 3. Gestion des Droits & Utilisateurs
| Commande | Description / Action |
| :--- | :--- |
| `chmod 755 fichier` | Modifie les permissions (rwxr-xr-x) |
| `chmod +x script.sh` | Rend un script exécutable |
| `chown util:groupe fichier` | Change le propriétaire et le groupe d'un fichier |
| `sudo useradd -m nouvel_util` | Crée un utilisateur avec son dossier `/home` |
| `sudo passwd nouvel_util` | Définit ou réinitialise le mot de passe d'un utilisateur |
| `sudo usermod -aG sudo util` | Ajoute un utilisateur au groupe `sudo` (sur Debian/Ubuntu) |
| `su - utilisateur` | Change d'utilisateur (ou passe en `root` sans argument) |
| `whoami` | Affiche le nom de l'utilisateur actuel |

---

## 4. Réseau & Sécurité
| Commande | Description / Action |
| :--- | :--- |
| `ip a` ou `ip addr` | Affiche les interfaces réseau et leurs adresses IP |
| `ip route` | Affiche la table de routage / passerelle par défaut |
| `ping -c 4 <IP/Nom>` | Envoie 4 requêtes ICMP pour tester la connectivité |
| `ss -tulnp` | Liste les ports en écoute avec les processus associés |
| `traceroute <IP/Nom>` | Suit le chemin des paquets vers une cible |
| `nslookup <Domaine>` | Interroge les serveurs DNS |
| `ufw status` | Affiche l'état du pare-feu UFW (Ubuntu/Debian) |
| `curl -I https://site.com` | Affiche les en-têtes HTTP d'un site web |

---

## 5. Gestion des Processus & Ressources
| Commande | Description / Action |
| :--- | :--- |
| `top` / `htop` | Gestionnaire de tâches interactif en temps réel |
| `ps aux` | Affiche tous les processus en cours d'exécution |
| `ps aux | grep service` | Filtre les processus par nom |
| `kill -9 <PID>` | Force l'arrêt d'un processus via son PID |
| `killall <nom_processus>` | Arrête tous les processus portant ce nom |
| `free -h` | Affiche l'utilisation de la mémoire RAM en format lisible (`-h`) |
| `df -h` | Affiche l'espace disque disponible sur les partitions montées |
| `du -sh /dossier` | Affiche la taille totale occupée par un dossier |

---

## 6. Services & Gestion de Paquets (Debian / Ubuntu)
| Commande | Description / Action |
| :--- | :--- |
| `sudo apt update` | Maintient la liste des paquets à jour |
| `sudo apt upgrade` | Met à jour tous les paquets installés |
| `sudo apt install paquet` | Installe un nouveau paquet |
| `systemctl status service` | Vérifie l'état d'un service (ex: `ssh`, `apache2`) |
| `sudo systemctl start service` | Démarre un service |
| `sudo systemctl stop service` | Arrête un service |
| `sudo systemctl restart service` | Redémarre un service |
| `sudo systemctl enable service` | Active le lancement automatique du service au démarrage |

---

## 7. Archivage & Compression
| Commande | Description / Action |
| :--- | :--- |
| `tar -czvf archive.tar.gz dossier/` | Compresse un dossier au format `.tar.gz` |
| `tar -xzvf archive.tar.gz` | Extrait une archive `.tar.gz` |
| `zip -r archive.zip dossier/` | Crée un fichier ZIP récursif |
| `unzip archive.zip` | Extrait un fichier ZIP |