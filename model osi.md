# Le Modèle OSI et ses Unités de Données de Protocole (PDU)

Le **modèle OSI** (*Open Systems Interconnection*) est un standard conceptuel développé par l'ISO (Organisation internationale de normalisation) pour décrire comment les informations d'une application logicielle se déplacent à travers un réseau physique vers une autre application.

Il est composé de **7 couches distinctes**, chacune réalisant une fonction spécifique et manipulant une unité de données particulière appelée **PDU** (*Protocol Data Unit*).

---

## Synthèse du Modèle OSI et des PDU

| Couche | Nom de la couche | PDU (Unité de données) | Rôle principal | Exemples de protocoles / Équipements |
| :---: | :--- | :--- | :--- | :--- |
| **7** | Application | **Données** (*Data*) | Interface réseau pour les applications utilisateur | HTTP, HTTPS, FTP, SMTP, DNS |
| **6** | Présentation | **Données** (*Data*) | Formate, chiffre et compresse les données | SSL/TLS, JPEG, ASCII, MPEG |
| **5** | Session | **Données** (*Data*) | Gère les sessions de communication | NetBIOS, RPC, PPTP |
| **4** | Transport | **Segment** (TCP) / **Datagramme** (UDP) | Connexion de bout en bout et contrôle de flux | TCP, UDP |
| **3** | Réseau | **Paquet** (*Packet*) | Adressage logique et routage des données | IP (IPv4/IPv6), ICMP, Routeurs |
| **2** | Liaison de données | **Trame** (*Frame*) | Adressage physique et accès au média | Ethernet, Wi-Fi (802.11), Switches |
| **1** | Physique | **Bit** | Transmission brute des signaux sur le support | Câbles RJ45, Fibre optique, Hubs |

---

## Détail par Couche

### 1. Couche Physique (*Physical*)
- **PDU :** **Bit**
- **Rôle :** Elle s'occupe de la transmission brute de données non structurées sous forme de signaux électriques, optiques ou radio sur un canal de communication physique.
- **Fonctions clés :** Codage des bits, vitesse de transmission, topologie physique, connecteurs.

### 2. Couche Liaison de Données (*Data Link*)
- **PDU :** **Trame** (*Frame*)
- **Rôle :** Elle assure le transfert fiable de données entre deux nœuds directement connectés sur le même réseau local (LAN).
- **Fonctions clés :** Adressage MAC, détection et correction d'erreurs au niveau physique, gestion des collisions.

### 3. Couche Réseau (*Network*)
- **PDU :** **Paquet** (*Packet*)
- **Rôle :** Elle gère le routage des données à travers plusieurs réseaux interconnectés (WAN/Internet).
- **Fonctions clés :** Adressage logique (adresses IP), sélection du meilleur chemin (routage).

### 4. Couche Transport (*Transport*)
- **PDU :** **Segment** (avec TCP) / **Datagramme** (avec UDP)
- **Rôle :** Elle assure la livraison de données de bout en bout entre processus applicatifs distants.
- **Fonctions clés :** Multiplexage via les numéros de ports, contrôle d'erreur, segmentation, réassemblage, contrôle de flux (TCP).

### 5. Couche Session (*Session*)
- **PDU :** **Données** (*Data*)
- **Rôle :** Elle ouvre, gère et ferme les sessions d'échange entre les applications hôtes.
- **Fonctions clés :** Synchronisation des échanges, gestion des dialogues (half-duplex, full-duplex).

### 6. Couche Présentation (*Presentation*)
- **PDU :** **Données** (*Data*)
- **Rôle :** Elle traduit les données du format réseau vers le format de l'application et inversement.
- **Fonctions clés :** Traduction de jeux de caractères (ex: ASCII, UTF-8), chiffrement/déchiffrement (ex: TLS/SSL), compression.

### 7. Couche Application (*Application*)
- **PDU :** **Données** (*Data*)
- **Rôle :** C'est la couche la plus proche de l'utilisateur. Elle fournit les services réseau directement aux applications.
- **Fonctions clés :** Transfert de fichiers, messagerie, navigation web, requêtes DNS.

---

## Le Processus d'Encapsulation et Décapsulation

Lors de l'envoi d'une information :
1. **Encapsulation (Émission) :** Les données descendent de la couche 7 à la couche 1. À chaque étape, la couche ajoute un en-tête (*header*) — et parfois un pied de page (*trailer*) — contenant des informations de contrôle.
   - **Données** → **Segment** → **Paquet** → **Trame** → **Bits**
2. **Décapsulation (Réception) :** Le destinataire reçoit les bits en couche 1 et remonte jusqu'à la couche 7 en retirant progressivement les en-têtes à chaque niveau pour restituer le message d'origine.

## 8. Le Modèle TCP/IP

Le modèle TCP/IP (plus simple et plus pragmatique que le modèle OSI) comporte 4 couches principales. Il est la base d'Internet tel qu'on le connaît aujourd'hui.

### Les 4 couches du modèle TCP/IP :

1. **Couche Application** (Correspond aux couches 5, 6 et 7 d'OSI)
   - **Rôle :** Regroupe les services réseau, la gestion des sessions et le formatage des données pour l'utilisateur.
   - **Protocoles phares :** HTTP, HTTPS, DNS, FTP, SMTP, DHCP.
   - **PDU :** Données (Data) / Message.

2. **Couche Transport** (Correspond à la couche 4 d'OSI)
   - **Rôle :** Assure la communication de bout en bout, le contrôle du flux et la correction des erreurs.
   - **Protocoles phares :** TCP (fiable, orienté connexion) et UDP (rapide, non orienté connexion).
   - **PDU :** Segment (pour TCP) ou Datagramme (pour UDP).

3. **Couche Internet** (Correspond à la couche 3 d'OSI - Réseau)
   - **Rôle :** Permet l'adressage logique des paquets et le routage à travers différents réseaux.
   - **Protocoles phares :** IP (IPv4 / IPv6), ICMP, ARP.
   - **PDU :** Paquet (Packet).

4. **Couche Accès Réseau / Liaison** (Correspond aux couches 1 et 2 d'OSI)
   - **Rôle :** Gère la transmission physique des données sur le support matériel et l'adressage physique (MAC).
   - **Protocoles phares :** Ethernet, Wi-Fi, PPP.
   - **PDU :** Trame (Frame) / Bits.

---

## 9. Tableau Comparatif : OSI vs TCP/IP

| Caractéristique | Modèle OSI | Modèle TCP/IP |
| :--- | :--- | :--- |
| **Nombre de couches** | 7 couches | 4 couches |
| **Approche** | Théorique et conceptuelle | Pragmatique et fonctionnelle |
| **Création** | Conçu avant les protocoles | Conçu après les protocoles (Internet) |
| **Flexibilité** | Strict sur la séparation des rôles | Plus souple |
| **Utilisation** | Modèle de référence académique / analyse | Utilisé concrètement par Internet |

### Correspondance directe des couches :
- **OSI 7, 6, 5** -> **TCP/IP : Application**
- **OSI 4** -> **TCP/IP : Transport**
- **OSI 3** -> **TCP/IP : Internet**
- **OSI 2, 1** -> **TCP/IP : Accès Réseau**