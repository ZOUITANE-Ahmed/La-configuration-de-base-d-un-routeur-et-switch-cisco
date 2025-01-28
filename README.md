### Commandes de Base pour Routeurs et Commutateurs

#### 1-Accès et Configuration
- **Mode privilégié :**
  ```bash
  Router> enable
  Switch> enable
  ```
- **Mode de configuration globale :**
  ```bash
  Router# configure terminal
  Switch# configure terminal
  ```

#### 2-Configuration de l'Hôte
- **Renommer l'hôte :**
  ```bash
  Router(config)# hostname NouveauNom
  Switch(config)# hostname NouveauNom
  ```

#### 3-Configuration d'Interface FastEthernet ipv4 et ipv6 
- **3-a-Interface FastEthernet ipv4 :**
  ```bash
  Router(config)# interface FastEthernet numéro
  Router(config-if)# ip address @ masque_de_sous_réseau
  Router(config-if)# no shutdown
  Router(config-if)# exit
  ```
- **3-b-Interface FastEthernet ipv6 :**
  ```bash
  Router(config)# interface FastEthernet numéro
  Router(config-if)# ipv6 address adresse_ipv6/préfixe
  Router(config-if)# no shutdown
  Router(config-if)# exit
  ```
### 3-1-Exemple de Configuration d'Interface FastEthernet ipv4 et ipv6 
- **3-1-a-Interface FastEthernet ipv4 :**
  ```bash
  Router(config)# interface FastEthernet 0/0
  Router(config-if)# ip address 192.168.1.1 255.255.255.0
  Router(config-if)# no shutdown
  Router(config-if)# exit
  ```
- **3-1-b-Pour afficher les informations d'une interface FastEthernet en ipv4 :**
  ```bash
  Router# show ip interface FastEthernet0/0
  ```
- **3-1-c-Interface FastEthernet ipv6 :**
  ```bash
  Router(config)# interface FastEthernet 0/0
  Router(config-if)# ipv6 address 2001:db8::1/64
  Router(config-if)# no shutdown
  Router(config-if)# exit
  ```
  - **3-1-d-Pour afficher les informations d'une interface FastEthernet en IPv6:**
  ```bash
  Router# show ipv6 interface FastEthernet0/0
  ```
  -------------------------------------------
  
Voici comment configurer une interface série sur un routeur Cisco, y compris l'attribution d'une adresse IP et la configuration du taux de signalisation :

### 4-Configuration d'une Interface Serial ipv4 et ipv6 
1. **Sélectionner l'Interface Série :**
   Remplacez `numéro` par le numéro de l'interface série (par exemple, `0/0`, `0/1`).
   ```bash
   Router(config)# interface serial numéro
   ```

2. **Attribuer une Adresse IP :**
   Remplacez `@` par l'adresse IP souhaitée et `masque` par le masque de sous-réseau (par exemple, `255.255.255.0`).
   ```bash
   Router(config-if)# ip address @ masque
   ```

3. **Configurer le Taux de Signalisation :**
   Cette commande est généralement utilisée pour les interfaces DCE (Data Communications Equipment) pour définir le taux de signalisation. Remplacez `nombre` par la valeur appropriée en bits par seconde (par exemple, `64000`).
   ```bash
   Router(config-if)# clock rate nombre
   ```

4. **Activer l'Interface :**
   Cette commande active l'interface.
   ```bash
   Router(config-if)# no shutdown
   ```

5. **Sortir de la Configuration de l'Interface :**
   Cette commande vous ramène en mode de configuration globale.
   ```bash
   Router(config-if)# exit
   ```

### 4-1-Exemple de Configuration d'Interface Serial ipv4 et ipv6 
Voici un exemple de séquence de commandes avec des valeurs spécifiques :
**4-1-a-Configuration d'une Interface Serial ipv4 :**
```bash
Router(config)# interface serial 0/0
Router(config-if)# ip address 192.168.1.1 255.255.255.0
Router(config-if)# clock rate 64000
Router(config-if)# no shutdown
Router(config-if)# exit
```
  
### 4-2-Exemple de Configuration d'Interface Serial ipv6
```bash
Router(config)# interface serial 0/0
Router(config-if)# ipv6 address 2001:cafe:2::2/64
Router(config-if)# no shutdown
Router(config-if)# exit
```
Pour afficher un résumé de toutes les interfaces sur un routeur Cisco, y compris les adresses IPv4 ou IPv6 et l'état opérationnel, vous pouvez utiliser la commande suivante :

### 4-3Commande pour Afficher un Résumé des Interfaces
```bash
Router# show ip interface brief
```

### 4-3-aInformations Fournies par la Commande
- **Interface :** Nom de l'interface (par exemple, FastEthernet0/0, Serial0/0).
- **IP Address :** Adresse IPv4 assignée à l'interface.
- **Status :** État opérationnel de l'interface (par exemple, "up" ou "down").
- **Protocol :** État du protocole (par exemple, "up" ou "down").

### Exemple de Sortie
Voici un exemple de sortie de la commande `show ip interface brief` :

```
Interface              IP Address      Status      Protocol
FastEthernet0/0       192.168.1.1     up          up
Serial0/0             2001:db8::1     up          up
FastEthernet0/1       unassigned      administratively down down
Serial0/1             192.168.2.1     down        down
```

### 4-3-bPour les Interfaces IPv6
Pour afficher un résumé des interfaces avec les adresses IPv6, utilisez la commande suivante :

```bash
Router# show ipv6 interface brief
```

### Exemple de Sortie pour IPv6
Voici un exemple de sortie de la commande `show ipv6 interface brief` :

```
Interface              IPv6 Address              Status      Protocol
Serial0/0             2001:db8::1/64            up          up
FastEthernet0/0       unassigned                 down        down
```

### Remarques
- Ces commandes vous permettent de vérifier rapidement l'état et la configuration des interfaces sur votre routeur, facilitant ainsi le dépannage et la gestion du réseau.



### 4-Configuration des Mots de Passe
- **4-1-Accès par terminal :**
  ```bash
  Router(config)# line console 0
  Router(config-line)# password mot_de_passe
  Router(config-line)# login
  Router(config-line)# exit
  ```
- **4-2-Accès par Telnet :**
  ```bash
  Router(config)# line vty 0 4
  Router(config-line)# password mot_de_passe
  Router(config-line)# login
  Router(config-line)# exit
  ```
- **4-3-Accès SSH :**
  ```bash
  Router(config)# line vty 0 4
  Router(config-line)# transport input ssh
  Router(config-line)# login local
  Router(config-line)# exit
  ```
- **4-4-Mot de passe privilégié (crypté) :**
  ```bash
  Router(config)# enable secret mot_de_passe
  ```
- **4-5-Crypter tous les mots de passe :**
  ```bash
  Router(config)# service password-encryption
  ```

### 5-Configuration du Bannière :**
  ```bash
  Router(config)# banner motd # le message #
  ```

### 6-Configuration du Domaine
```bash
Router(config)# ip domain-name exemple.ma
```

### 7-Génération de Clés RSA pour SSH
```bash
Router(config)# crypto key generate rsa
Router(config)# ip ssh version 2
```

### 8-Création et Attribution de VLAN
- **8-1-Création de VLAN :**
  ```bash
  Switch# configure terminal
  Switch(config)# vlan vlan-id
  Switch(config-vlan)# name vlan-name
  Switch(config-vlan)# end
  ```
- **8-2-Attribution de Port à des VLAN :**
  ```bash
  Switch# configure terminal
  Switch(config)# interface interface-id
  Switch(config-if)# switchport mode access
  Switch(config-if)# switchport access vlan vlan-id
  Switch(config-if)# exit
  ```
### 9-commandes de vérification

  Voici un tableau récapitulatif des commandes de vérification sur un commutateur Cisco, avec les tâches et les commandes associées :

| **Tâche**                                               | **Commande IOS**                        |
|--------------------------------------------------------|-----------------------------------------|
| Affichez la configuration initiale actuelle            | `Switch# show startup-config`          |
| Affichez la configuration courante                      | `Switch# show running-config`          |
| Affichez les informations sur le système de fichiers Flash | `Switch# show flash`                  |
| Affichez l'état matériel et logiciel du système        | `Switch# show version`                 |
| Affichez l'historique des commandes exécutées          | `Switch# show history`                 |
| Affichez la table d'adresses MAC                       | `Switch# show mac-address-table` ou `Switch# show mac address-table`      |

### Remarques :
- **show startup-config** : Montre la configuration sauvegardée lors du dernier redémarrage.
- **show running-config** : Montre la configuration actuelle en cours d'exécution sur le commutateur.
- **show flash** : Affiche des informations sur le système de fichiers Flash, y compris l'espace libre et les fichiers stockés.
- **show version** : Fournit des informations sur le matériel et le logiciel, y compris la version IOS.
- **show history** : Affiche l'historique des commandes que l'utilisateur a exécutées.
- **show mac-address-table** ou **show mac address-table** : Affiche la table des adresses MAC, montrant les adresses MAC apprises et les ports correspondants.


Voici un résumé structuré des informations que vous avez fournies concernant DTP, VTP, et la configuration des trunks sur des commutateurs Cisco :


---

####
Voici les principales commandes HSRP (Hot Standby Router Protocol) que vous pouvez utiliser sur un routeur Cisco pour configurer et gérer HSRP :

### 1. **Configuration de base** :
Pour configurer HSRP sur une interface spécifique :

```bash
interface [interface]
 ip address [adresse_IP] [masque_sous_reseau]
 standby [groupe] ip [adresse_IP_virtuelle]
 standby [groupe] priority [priorité]
 standby [groupe] preempt
```

### 2. **Détails de la configuration** :
- **`[interface]`** : l'interface sur laquelle HSRP est configuré (ex. `GigabitEthernet0/1`).
- **`[adresse_IP]`** : l'adresse IP de l'interface.
- **`[masque_sous_reseau]`** : le masque de sous-réseau.
- **`[groupe]`** : le numéro de groupe HSRP (généralement entre 1 et 255).
- **`[adresse_IP_virtuelle]`** : l'adresse IP partagée par le groupe HSRP.
- **`[priorité]`** : la priorité du routeur (valeur par défaut : 100).

### 3. **Activer le préemption** :
Pour activer le préemption, ce qui permet au routeur ayant la plus haute priorité de reprendre le rôle de routeur actif :

```bash
standby [groupe] preempt
```

### 4. **Afficher l'état HSRP** :
Pour afficher l'état HSRP sur une interface :

```bash
show standby
```

### 5. **Afficher les informations de groupe HSRP** :
Pour afficher les détails des groupes HSRP :

```bash
show standby brief
```

### 6. **Débogage HSRP** :
Pour déboguer les messages HSRP :

```bash
debug standby
```

### Exemples de configuration :

**Exemple de configuration HSRP sur deux routeurs :**

#### Routeur A :
```bash
interface GigabitEthernet0/1
 ip address 192.168.1.1 255.255.255.0
 standby 1 ip 192.168.1.254
 standby 1 priority 110
 standby 1 preempt
```

#### Routeur B :
```bash
interface GigabitEthernet0/1
 ip address 192.168.1.2 255.255.255.0
 standby 1 ip 192.168.1.254
 standby 1 priority 100
 standby 1 preempt
```





Dans cet exemple, le routeur A aura la priorité la plus élevée et sera le routeur actif. Si le routeur A tombe en panne, le routeur B prendra le relais.


L'inter-VLAN (Virtual Local Area Network) est une méthode permettant la communication entre différents VLANs sur un réseau. Pour mettre en place l'inter-VLAN sur un routeur et un switch, voici les étapes principales en français :

### 1. **Configuration sur le Switch :**
   - **Créer les VLANs** : Commence par créer les VLANs sur le switch.
     ```bash
     Switch(config)# vlan 10
     Switch(config-vlan)# name Sales
     Switch(config-vlan)# exit
     
     Switch(config)# vlan 20
     Switch(config-vlan)# name Marketing
     Switch(config-vlan)# exit
     ```
   - **Assigner les ports aux VLANs** :
     Chaque port correspondant à une machine doit être associé à un VLAN.
     ```bash
     Switch(config)# interface FastEthernet 0/1
     Switch(config-if)# switchport mode access
     Switch(config-if)# switchport access vlan 10
     Switch(config-if)# exit
     
     Switch(config)# interface FastEthernet 0/2
     Switch(config-if)# switchport mode access
     Switch(config-if)# switchport access vlan 20
     Switch(config-if)# exit
     ```

   - **Configurer le Trunk** : Le port connecté au routeur doit être en mode trunk pour permettre la transmission des VLANs.
     ```bash
     Switch(config)# interface FastEthernet 0/24
     Switch(config-if)# switchport mode trunk
     Switch(config-if)# switchport trunk encapsulation dot1q
     Switch(config-if)# exit
     ```

### 2. **Configuration sur le Routeur :**
   Sur le routeur, il est nécessaire de configurer des sous-interfaces pour chaque VLAN afin de permettre l'inter-VLAN routing.

   - **Créer des sous-interfaces pour chaque VLAN** :
     ```bash
     Router(config)# interface GigabitEthernet 0/0.10
     Router(config-subif)# encapsulation dot1Q 10
     Router(config-subif)# ip address 192.168.10.1 255.255.255.0
     Router(config-subif)# exit
     
     Router(config)# interface GigabitEthernet 0/0.20
     Router(config-subif)# encapsulation dot1Q 20
     Router(config-subif)# ip address 192.168.20.1 255.255.255.0
     Router(config-subif)# exit
     ```

   - **Activer l'interface physique** :
     ```bash
     Router(config)# interface GigabitEthernet 0/0
     Router(config-if)# no shutdown
     ```

### 3. **Test de connectivité** :
   Une fois la configuration terminée, teste la communication entre les VLANs en pingant une machine d'un VLAN depuis une machine d'un autre VLAN. Si tout est correctement configuré, les machines devraient pouvoir communiquer.

### Résumé :
- Sur le switch : créer les VLANs, assigner les ports aux VLANs et configurer un port trunk.
- Sur le routeur : créer des sous-interfaces pour chaque VLAN avec encapsulation dot1Q pour l'inter-VLAN routing.

Cela permet d'avoir un réseau segmenté tout en assurant la communication entre les différents segments grâce au routeur.


-----------------------------------------------------------------------

L'inter-VLAN (ou routage inter-VLAN) est un processus qui permet la communication entre des périphériques situés dans des VLANs différents. Les VLANs (réseaux locaux virtuels) segmentent logiquement un réseau en domaines de diffusion distincts, améliorant ainsi la sécurité et les performances réseau. Cependant, pour que des périphériques dans des VLANs distincts puissent communiquer, un routeur ou un switch de couche 3 est nécessaire.

### Principaux Composants du Routage Inter-VLAN

1. **Router-on-a-Stick** : Une méthode courante consiste à utiliser une interface physique unique sur un routeur, configurée en mode trunk pour gérer plusieurs VLANs. Le routeur crée des sous-interfaces pour chaque VLAN, auxquelles il attribue des adresses IP, permettant ainsi le routage entre eux.

2. **Switchs de Couche 3** : Ces switches combinent des capacités de routage et de commutation dans un seul appareil, permettant le routage inter-VLAN directement. Chaque VLAN reçoit une interface virtuelle séparée (SVI - Switch Virtual Interface) sur le switch de couche 3, qui sert de passerelle par défaut pour les appareils au sein de ce VLAN.

3. **Configuration de Routage** :
   - **Sous-interfaces** : Pour un router-on-a-stick, chaque VLAN obtient une sous-interface sur le routeur avec sa propre adresse IP et son balisage VLAN (tagging).
   - **Ports Trunks** : Les ports trunk sur les switches transportent le trafic de plusieurs VLANs. Le routeur ou le switch de couche 3 doit être connecté via un port trunk pour gérer le trafic des VLANs étiquetés.

4. **Adresse de Passerelle** : Chaque VLAN dispose de sa propre adresse IP de passerelle par défaut, généralement configurée sur le routeur ou le switch de couche 3 pour permettre la communication en dehors du VLAN.

### Étapes de Configuration (Router-on-a-Stick)

1. **Créer les VLANs sur le switch** :
   ```plaintext
   Switch(config)# vlan 10
   Switch(config-vlan)# name Vente
   Switch(config)# vlan 20
   Switch(config-vlan)# name Marketing
   ```

2. **Configurer le trunk sur le port du switch connecté au routeur** :
   ```plaintext
   Switch(config)# interface gig0/1
   Switch(config-if)# switchport mode trunk
   ```

3. **Configurer les sous-interfaces sur le routeur** :
   ```plaintext
   Router(config)# interface gig0/0.10
   Router(config-subif)# encapsulation dot1Q 10
   Router(config-subif)# ip address 192.168.10.1 255.255.255.0

   Router(config)# interface gig0/0.20
   Router(config-subif)# encapsulation dot1Q 20
   Router(config-subif)# ip address 192.168.20.1 255.255.255.0
   ```

4. **Tester la Connectivité** : Vérifiez que les appareils dans chaque VLAN peuvent pinger leur passerelle par défaut et se connecter entre eux via le routage inter-VLAN.

Grâce au routage inter-VLAN, les différents VLANs peuvent communiquer de manière sécurisée tout en conservant la segmentation logique du réseau.

-------------------------------------------------------------------------------------------------------------
Voici les propriétés, les avantages et les commandes des protocoles FHRP (First Hop Redundancy Protocols) : **VRRP** (Virtual Router Redundancy Protocol), **HSRP** (Hot Standby Router Protocol) et **GLBP** (Gateway Load Balancing Protocol).

### 1. **VRRP (Virtual Router Redundancy Protocol)**
#### Propriétés
- **Normalisé** : VRRP est un protocole standardisé par l'IETF (RFC 5798).
- **Adressage IP Virtuel** : Il permet à plusieurs routeurs de partager une adresse IP virtuelle, où l'un des routeurs est élu comme maître.

#### Avantages
- **Interopérabilité** : Puisqu'il est standardisé, il fonctionne avec différents équipements de différents fabricants.
- **Basé sur les priorités** : La redondance est déterminée par la priorité, et le routeur ayant la plus haute priorité devient le maître.

#### Commandes de base (Cisco IOS)
```bash
interface GigabitEthernet0/1
  vrrp 1 ip 192.168.1.1
  vrrp 1 priority 120
  vrrp 1 preempt
```

### 2. **HSRP (Hot Standby Router Protocol)**
#### Propriétés
- **Propriétaire** : HSRP est un protocole propriétaire de Cisco.
- **État de veille actif/secondaire** : Un routeur est actif, tandis que l'autre reste en mode veille, prêt à prendre la relève si nécessaire.

#### Avantages
- **Facile à configurer** : Idéal pour les réseaux simples où une redondance basique est nécessaire.
- **Suivi d'interface** : Possibilité de suivre les interfaces pour détecter les pannes de routeur et basculer automatiquement vers le routeur de secours.

#### Commandes de base (Cisco IOS)
```bash
interface GigabitEthernet0/1
  standby 1 ip 192.168.1.1
  standby 1 priority 110
  standby 1 preempt
```

### 3. **GLBP (Gateway Load Balancing Protocol)**
#### Propriétés
- **Propriétaire** : GLBP est également un protocole propriétaire de Cisco.
- **Équilibrage de charge** : GLBP permet l'équilibrage de charge entre plusieurs routeurs, en répartissant le trafic sur plusieurs passerelles.

#### Avantages
- **Distribution du trafic** : Capacité à utiliser plusieurs passerelles de manière simultanée, améliorant ainsi la répartition du trafic.
- **Gestion de l'état de veille** : Comme HSRP, GLBP prend en charge la bascule en cas de panne, tout en permettant l'équilibrage de charge.

#### Commandes de base (Cisco IOS)
```bash
interface GigabitEthernet0/1
  glbp 1 ip 192.168.1.1
  glbp 1 priority 100
  glbp 1 preempt
```

Ces protocoles permettent tous d'assurer la redondance et la haute disponibilité dans les réseaux, mais ils diffèrent par leur fonctionnement, leurs avantages et leurs options de configuration.

