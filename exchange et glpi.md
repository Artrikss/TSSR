# Fiche de Révision TSSR : Microsoft Exchange & Collecteur GLPI
 

---

## Partie 1 : Installation et Administration de Microsoft Exchange Server

### 1. Présentation générale
* **Définition :** Système de messagerie et de collaboration développé par Microsoft (emails, calendriers partagés, contacts, tâches).
* **Outils d'administration :**
  * **EAC / ECP** (*Exchange Admin Center / Exchange Control Panel*) : Interface web d'administration (`https://<IP_ou_Nom_Serveur>/ecp`).
  * **OWA** (*Outlook Web App*) : Client web de messagerie pour les utilisateurs (`https://<IP_ou_Nom_Serveur>/owa`).
  * **EMS** (*Exchange Management Shell*) : Administration en ligne de commande via PowerShell.
* **Sécurité :** Utilisation de **SSL/TLS**, authentification multifacteur (MFA), chiffrement des emails, et filtrage anti-spam/anti-malware (EOP : filtrage de connexion, anti-malware, règles de flux, filtrage de contenu).

---

### 2. Prérequis matériels et infrastructure (Lab 2 VMs)

| Composant | VM 1 : Contrôleur de Domaine (DC) | VM 2 : Serveur Exchange |
| :--- | :--- | :--- |
| **OS** | Windows Server 2025 | Windows Server 2025 |
| **CPU / RAM** | 4 cœurs / 8 Go RAM min. | 4 cœurs / 8 Go RAM min. *(16 Go + 8 cœurs conseillés pour accélérer l'installation)* |
| **Disque** | 50 Go | 100 Go |
| **Réseau** | Carte NAT | Carte NAT (intégrée au domaine AD) |
| **Rôles** | AD DS + DNS (promu DC) | Rôle de boîte aux lettres Exchange |

---

### 3. Prérequis logiciels sur le serveur Exchange
Avant de lancer l'installation d'Exchange (`ExchangeServerSE-x64.iso`), il faut installer **3 packages obligatoires** (sinon le test de préparation bloque) :
1. **Visual C++ 2013 Redistributable (x64)** (`vcredist_x64.exe`)
2. **Microsoft Unified Communications Managed API 4.0 Runtime** (`UcmaRuntimeSetup.exe`)
3. **Module de réécriture d'URL 2 d'IIS** (`rewrite_amd64_fr-FR.msi`)

---

### 4. Étapes d'installation d'Exchange
1. **Connexion :** Se connecter sur la VM Exchange avec le compte Administrateur du domaine (`NETBIOS\Administrateur`).
2. **Lancement :** Monter l'ISO `ExchangeServerSE-x64.iso` et exécuter `Setup.exe`.
3. **Assistant d'installation :**
   * Accepter le contrat de licence.
   * Sélectionner *Ne pas utiliser les paramètres recommandés*.
   * Cocher **Rôle de boîte aux lettres** et cocher **Installer automatiquement les rôles et fonctionnalités Windows Server requis**.
   * Conserver le chemin par défaut (`C:\Program Files\Microsoft\Exchange Server\V15`).
   * Laisser la protection anti-malware activée (*Désactiver : Non*).
4. **Tests de préparation :** Vérifier que les prérequis passent à 100 % (l'avertissement concernant l'absence de connecteur d'envoi `*` vers Internet est normal en lab local).
5. **Installation (13 étapes / ~45 min) :** Pendant l'installation, créer les utilisateurs dans **Utilisateurs et ordinateurs Active Directory**.
6. **Finalisation :** Redémarrer le serveur une fois l'installation terminée et **faire un snapshot des VMs**.

---

### 5. Post-installation et accès web
* **Administration (ECP) :** `https://<IP_Serveur>/ecp` -> Connexion avec `NETBIOS\Administrateur` ou `Administrateur@domaine.local`. Paramétrer le fuseau horaire à la première connexion.
* **Messagerie client (OWA) :** `https://<IP_Serveur>/owa` -> Permet de tester l'envoi et la réception d'emails entre les utilisateurs du domaine.

---

## Partie 2 : Collecteur GLPI (Création de tickets par e-mail)

### 1. Principe de fonctionnement
Un collecteur permet à GLPI de relever automatiquement une boîte mail dédiée (ex. `support@domaine.local`) pour transformer chaque message reçu en ticket d'assistance :
* **Objet du mail** $\rightarrow$ **Titre du ticket**
* **Corps du mail** $\rightarrow$ **Description du ticket**

---

### 2. Création de la boîte mail Support (sur Exchange)
1. Se connecter à l'ECP (`https://<IP_Serveur>/ecp`).
2. Aller dans **Destinataires** > **Boîtes aux lettres** > **+** (Nouvelle boîte aux lettres utilisateur).
3. Renseigner les informations :
   * **Nom / Nom d'affichage :** `Support GLPI`
   * **Alias / Nom d'ouverture de session :** `support@domaine.local`
   * Décocher *Exiger la modification du mot de passe à la prochaine ouverture de session*.
   * Optionnel : Cocher *Masqué dans les listes d'adresses*.
4. Noter précieusement l'adresse mail et le mot de passe.

---

### 3. Prérequis côté GLPI
1. **Autoriser les tickets/suivis anonymes :**
   * Aller dans **Configuration** > **Générale** > onglet **Assistance**.
   * Passer à **Oui** : *Autoriser les suivis anonymes (collecteur)* et *Autoriser les ouvertures de tickets anonymes (collecteur)*.
2. **Importation LDAP :**
   * Aller dans **Administration** > **Utilisateurs** > **Liaison annuaire LDAP** > **Importer des utilisateurs** afin d'importer le compte `support@domaine.local` depuis l'Active Directory.

---

### 4. Activation des services POP3 / IMAP4 (sur le serveur Exchange)
Par défaut sur Exchange, les services de récupération POP et IMAP sont arrêtés ou en démarrage manuel.
1. Ouvrir la console des services (`Win + R` $\rightarrow$ `services.msc`).
2. Configurer les **4 services suivants** en type de démarrage **Automatique** et cliquer sur **Démarrer** :
   * `Microsoft Exchange IMAP4`
   * `IMAP4 principal de Microsoft Exchange`
   * `Microsoft Exchange POP3`
   * `POP3 principal Microsoft Exchange`

---

### 5. Configuration du Collecteur dans GLPI
1. Aller dans **Configuration** > **Collecteurs** > **+ Ajouter**.
2. Renseigner les paramètres suivants :
   * **Nom :** `support@domaine.local`
   * **Actif :** `Oui`
   * **Serveur :** `<IP_du_serveur_Exchange>` (ex. `172.16.4.201`)
   * **Options de connexion :** `POP` | `TLS` | `NO-VALIDATE-CERT` *(génère la chaîne `{IP/pop/novalidate-cert/tls}`)*
   * **Port (optionnel) :** `110`
   * **Identifiant :** `support@domaine.local` (ou `support`)
   * **Mot de passe :** Mot de passe du compte support
3. Cliquer sur **Sauvegarder**.
4. Aller dans l'onglet **Actions** (en haut à droite) > **Récupérer les emails maintenant** pour vérifier que la connexion s'établit sans erreur.

---

### 6. Test et automatisation (`mailgate`)
1. **Envoyer un mail de test :** Depuis un client (OWA ou Outlook) connecté avec un compte utilisateur, envoyer un email à `support@domaine.local`.
2. **Forcer ou planifier la relève :**
   * Aller dans **Configuration** > **Actions automatiques** > rechercher **`mailgate`**.
   * Vérifier que le statut est sur **Programmée** (fréquence par défaut : 10 minutes, mode GLPI).
   * Cliquer sur **Exécuter** pour déclencher la relève immédiatement.
3. **Vérification :** Aller dans **Assistance** > **Tickets** pour constater l'apparition du nouveau ticket avec la source *E-Mail*.