# Fiche de Révision : Les Notions Clés de l'Informatique & Réseaux

## 1. Réseaux & Protocoles

* **Modèle OSI** : Modèle théorique qui décrit comment l'information circule à travers un réseau, de l'application jusqu'au câble physique.
* **TCP/IP** : Ensemble des protocoles fondamentaux utilisés pour la communication et l'échange de données sur Internet et les réseaux locaux.
* **Adresse IP & Adresse MAC** :
  * **Adresse MAC** : Adresse physique unique attribuée à la carte réseau d'un appareil.
  * **Adresse IP** : Adresse logique qui permet d'identifier et de localiser un appareil sur un réseau.
* **DNS (Domain Name System)** : Service qui traduit les noms de domaine en adresses IP (ex. `google.com` $\rightarrow$ `8.8.8.8`) pour faciliter la navigation.
* **DHCP (Dynamic Host Configuration Protocol)** : Protocole qui attribue automatiquement des adresses IP aux équipements du réseau.
  * **Étendue (Scope)** : Plage d'adresses IP attribuables par le serveur DHCP (ex. de `192.168.85.10` à `192.168.85.20`).
  * **Bail (Lease)** : Durée de validité pendant laquelle une adresse IP est réservée pour un équipement avant d'être renouvelée ou réattribuée.
* **Normes IEEE** :
  * **IEEE 802.3** : Norme pour le réseau filaire (**Ethernet**).
  * **IEEE 802.11** : Norme pour le réseau sans fil (**Wi-Fi**).
* **Modes de Réseau Virtuel** :
  * **Bridge (Pont)** : La machine virtuelle obtient sa propre adresse IP sur le réseau physique, comme un PC indépendant.
  * **NAT** : La machine virtuelle partage l'adresse IP de l'hôte pour accéder au réseau extérieur.
  * **Host-Only (Hôte privé)** : La machine virtuelle communique uniquement avec l'hôte et les autres VMs du même réseau fermé, sans accès Internet.

---

## 2. Déploiement & Système d'Exploitation

* **ISO** : Fichier image contenant l'intégralité d'un support de stockage (ex. l'installateur d'un système d'exploitation).
* **PXE (Preboot Execution Environment)** : Protocole permettant à un ordinateur de démarrer et d'installer un système d'exploitation à travers le réseau via un serveur.
* **WDS (Windows Deployment Services)** : Service Microsoft permettant de déployer des systèmes d'exploitation à distance via le réseau (en s'appuyant souvent sur PXE), sans utiliser de clé USB.
* **Rufus / Ventoy** : Outils logiciels servant à créer des clés USB bootables (démarrables) à partir de fichiers ISO.
* **Windows Server** : Version du système d'exploitation Windows conçue pour fournir des services réseau (DHCP, DNS, Active Directory, partage de fichiers) à grande échelle.

---

## 3. Matériel & Stockage

* **Octets & Bits** : Unités fondamentales de mesure en informatique. Un **byte/octet** équivaut à 8 **bits**. Le bit mesure généralement le débit (ex. Mbit/s), tandis que l'octet mesure la capacité de stockage (ex. Go).
* **BIOS / UEFI** : Micrologiciel (firmware) présent sur la carte mère qui initialise le matériel et démarre le système d'exploitation. L'**UEFI** est le successeur moderne du BIOS, offrant une interface graphique et la prise en charge de disques plus grands.
* **Tables de Partition (MBR / GPT)** :
  * **MBR (Master Boot Record)** : Ancienne structure de partition (limitée à 2 To et 4 partitions principales).
  * **GPT (GUID Partition Table)** : Structure moderne associée à l'UEFI, sans restriction pratique de taille ou de nombre de partitions.
* **Partitionnement** : Action de diviser un disque dur physique en plusieurs sections logiques indépendantes (ex. séparer le système $C:$ des données $D:$).
* **NTFS (New Technology File System)** : Système de fichiers propriétaire de Microsoft offrant sécurité, gestion des droits d'accès et tolérance aux pannes.
* **SHA-256** : Algorithme de hachage qui génère l'empreinte numérique unique d'un fichier. Permet de vérifier l'intégrité d'un fichier (s'assurer qu'il n'a pas été altéré ou corrompu).

---

## 4. Virtualisation

* **Hyperviseur** : Logiciel permettant de créer et de gérer des machines virtuelles (VMs).
* **Type 1 (Bare-metal / Nu-propriétaire)** : S'installe directement sur le matériel physique sans système d'exploitation intermédiaire (ex. **VMware ESXi**). Offre de meilleures performances.
* **Type 2 (Hébergé)** : S'installe comme un logiciel classique au-dessus d'un système d'exploitation hôte déjà existant (ex. **VMware Workstation**, VirtualBox).

---

## 5. Gestion de Parc & Outils IT

* **Gestionnaire de Parc Informatique (Gestion de P.I. / ITSM)** : Outil ou logiciel (ex. GLPI) permettant d'inventorier, de suivre et de gérer l'ensemble des équipements informatiques (ordinateurs, serveurs, logiciels, tickets d'incidents) d'une entreprise.