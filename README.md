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

### DTP (Dynamic Trunking Protocol)
- **Description :** Protocole propriétaire de Cisco utilisé pour la négociation des trunks entre les commutateurs.
- **Fonctionnalité :** Permet de créer ou de supprimer dynamiquement des trunks selon la configuration des ports.

### VTP (VLAN Trunking Protocol)
- **Description :** Facilite la gestion des VLANs dans un réseau.
- **Fonctionnalité :** Propague les informations de VLAN dans le réseau entre les commutateurs connectés via trunks.

### Trunking
- **ISL (Inter-Switch Link) :**
  - Protocole d'encapsulation propriétaire de Cisco.
  - Moins utilisé aujourd'hui en raison de sa spécificité.
  - Ne supporte pas VLAN Natif.

- **dot1q (IEEE 802.1Q) :**
  - Norme ouverte utilisée pour le trunking dans des environnements multi-fournisseurs.
  - Préférée pour son interopérabilité entre différents équipements réseau.
  - Supporte le VLAN Natif.

### Modes de Port dans DTP
1. **Access :** Le port appartient à un seul VLAN, sans négociation de trunk.
2. **Trunk :** Le port transporte le trafic de plusieurs VLANs.
3. **Dynamic Auto :** Tente de devenir un trunk en envoyant des paquets DTP.
4. **Dynamic Desirable :** Peut devenir un trunk mais n'initie pas la négociation.

### Résumé des Résultats de la Négociation DTP entre Switches (Sw1 et Sw2)

|                                    | **sw2 : Dynamic Auto**             | **sw2 : Dynamic Desirable**          | **sw2 : Trunk**    | **sw2 : Access**   |
|------------------------------------|------------------------------------|--------------------------------------|-----------------------------------------|
| **sw1 : Dynamic Auto**             | Access                             | Trunk                                |   Trunk            | Access             |
| **sw1 : Dynamic Desirable**        | Trunk                              | Trunk                                |                    |                    |
| **sw1 : Trunk**                    | Dynamic Auto                       | Trunk                                |                    |                    |
| **sw1 : Access**                   | Access                             | Access                               |                    |                    |


### Configuration DTP
Pour configurer un port en mode trunk :
```bash
Switch1(config-if)# interfac f0/1
Switch1(config-if)# switchport mode trunk
Switch1(config-if)# exit
```

Pour vérifier le trunking :
```bash
Switch1# show interfaces trunk
```

#### Désactivation du Protocole DTP
Pour désactiver le protocole DTP sur un port :
```bash
Switch1(config)# interface f0/1
Switch1(config-if)# switchport mode trunk
Switch1(config-if)# switchport nonegotiate
```

#### Activation du Protocole DTP
Pour réactiver le protocole DTP :
```bash
Switch1(config-if)# no switchport nonegotiate
```

Pour afficher l'état et la configuration d'une interface spécifique :
```bash
Switch1# show interface fastethernet 0/1 switchport
```

### Fonctionnement du Protocole VTP
- **Domaines VTP :** Groupes de commutateurs partageant une base de VLANs.

#### Modes VTP :
1. **Server :** Peut créer, modifier et supprimer des VLANs.
2. **Client :** Reçoit les informations des VLANs sans modification.
3. **Transparent :** Ne propage pas les informations de VLANs, mais continue à transmettre les trames.

### Configuration VTP
Pour configurer un switch en mode serveur VTP :
```bash
Switch1(config)# vtp mode server
Switch1(config)# vtp domain ofppt.local
Switch1(config)# vtp password 2244
```

### Vérifier la Configuration VTP
Pour vérifier l'état VTP :
```bash
Switch1# show vtp status
```

---

Si vous avez besoin de plus d'informations ou d'autres clarifications, n'hésitez pas à demander !
