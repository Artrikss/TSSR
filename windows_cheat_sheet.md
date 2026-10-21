# Essential Windows Shortcuts & Commands Cheat Sheet

Une fiche de révision rapide pour les raccourcis clavier Windows les plus utiles, la gestion des fenêtres et les commandes système.

## 🚀 Raccourcis Généraux & Système

| Raccourci | Action | 
| ----- | ----- | 
| `Win` | Ouvrir ou fermer le menu Démarrer | 
| `Win + X` | Ouvrir le menu Liens Rapides (Gestionnaire de périphériques, Gestion des disques, Terminal, etc.) | 
| `Win + I` | Ouvrir les Paramètres Windows | 
| `Win + R` | Ouvrir la boîte de dialogue **Exécuter** | 
| `Win + L` | Verrouiller la session / le PC | 
| `Win + D` | Afficher ou masquer le bureau | 
| `Ctrl + Shift + Esc` | Ouvrir directement le Gestionnaire des tâches | 
| `Win + X` puis `A` | Ouvrir le Terminal / PowerShell en tant qu'administrateur | 

## 🖥️ Gestion des Fenêtres & Bureaux Virtuels

| Raccourci | Action | 
| ----- | ----- | 
| `Win + Ancre Gauche / Droite` | Ancrer la fenêtre active sur la moitié gauche ou droite de l'écran | 
| `Win + Ancre Haut / Bas` | Agrandir ou réduire la fenêtre active | 
| `Win + Tab` | Ouvrir l'affichage des tâches (Task View) | 
| `Ctrl + Win + Gauche / Droite` | Basculer d'un bureau virtuel à un autre | 
| `Ctrl + Win + D` | Créer un nouveau bureau virtuel | 
| `Alt + Tab` | Basculer entre les applications ouvertes | 

## 📁 Raccourcis Explorateur de Fichiers

| Raccourci | Action | 
| ----- | ----- | 
| `Win + E` | Ouvrir l'Explorateur de fichiers | 
| `Ctrl + Shift + N` | Créer un nouveau dossier | 
| `Alt + Enter` | Ouvrir les propriétés de l'élément sélectionné | 
| `Ctrl + Shift + T` | Rouvrir le dernier onglet fermé dans l'Explorateur | 
| `Alt + D` ou `Ctrl + L` | Sélectionner la barre d'adresse | 

## 🛠️ Commandes "Exécuter" Utiles (`Win + R`)

| Commande | Cible / Outil | 
| ----- | ----- | 
| `appwiz.cpl` | Programmes et fonctionnalités (Désinstaller un programme) | 
| `ncpa.cpl` | Connexions réseau | 
| `sysdm.cpl` | Propriétés système (Nom de l'ordinateur, variables d'environnement) | 
| `compmgmt.msc` | Gestion de l'ordinateur (Disques, services, utilisateurs locaux) | 
| `devmgmt.msc` | Gestionnaire de périphériques | 
| `gpedit.msc` | Éditeur de stratégie de groupe locale (Windows Pro/Enterprise) | 
| `regedit` | Éditeur du Registre | 

---

### Diagnostic Réseau & Sécurité (Invite de commande / PowerShell)
| Commande | Action / Utilité |
| :--- | :--- |
| `ipconfig /all` | Affiche la configuration réseau détaillée (MAC, DHCP, DNS, IP) |
| `ipconfig /flushdns` | Vide le cache DNS local (utile en cas de problème de résolution de nom) |
| `ping -t <IP_ou_Nom>` | Envoie des requêtes ICMP en continu pour tester la connectivité |
| `tracert <IP_ou_Nom>` | Affiche la route et les sauts (routeurs) pour atteindre une cible |
| `netstat -ano` | Liste toutes les connexions réseau actives avec leurs ports et PID |
| `nslookup <Domaine>` | Interroge le serveur DNS pour obtenir l'adresse IP d'un domaine |
| `getmac` | Affiche les adresses physiques (MAC) des cartes réseau |

---

### Outils de Diagnostic & Reparation Système (CMD / PowerShell en Administrateur)
| Commande | Action / Utilité |
| :--- | :--- |
| `sfc /scannow` | Analyse et restaure les fichiers système Windows corrompus |
| `DISM /Online /Cleanup-Image /RestoreHealth` | Répare l'image de Windows via Windows Update |
| `chkdsk C: /f /r` | Vérifie et répare les erreurs sur le disque C: (nécessite un redémarrage) |
| `tasklist` | Liste tous les processus en cours d'exécution avec leur PID |
| `taskkill /PID <PID> /F` | Force la fermeture d'un processus spécifique par son PID |

---

### Raccourcis Terminal & Invite de commande (CMD / PowerShell)
| Raccourci | Action |
| :--- | :--- |
| `Ctrl + C` | Interrompt la commande en cours d'exécution |
| `Flèche Haut / Bas` | Navigue dans l'historique des commandes saisies |
| `Tab` | Autocomplète le nom d'un fichier, dossier ou commande |
| `cls` (ou `Clear-Host`) | Efface le contenu de l'écran du terminal |




