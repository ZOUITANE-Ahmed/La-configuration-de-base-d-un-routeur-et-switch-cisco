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
Voici le tableau récapitulatif des résultats de la négociation du **Dynamic Trunking Protocol (DTP)** entre deux commutateurs (**sw1** et **sw2**) avec les différents modes de ports :

|                                    | **sw2 : Dynamic Auto**             | **sw2 : Dynamic Desirable**          | **sw2 : Trunk**    | **sw2 : Access**   |
|------------------------------------|------------------------------------|--------------------------------------|--------------------|--------------------|
| **sw1 : Dynamic Auto**             | Access                             | Trunk                                | Trunk              | Access             |
| **sw1 : Dynamic Desirable**        | Trunk                              | Trunk                                | Trunk              | Access             |
| **sw1 : Trunk**                    | Trunk                              | Trunk                                | Trunk              | Connectivité limitée (Non recommandé) |
| **sw1 : Access**                   | Access                             | Access                               | Connectivité limitée (Non recommandé) | Access             |

### Interprétation :
- **Dynamic Auto avec Dynamic Auto :** Le port ne devient pas un trunk, donc ils restent en mode Access.
- **Dynamic Auto avec Dynamic Desirable :** Le port devient un trunk car **Dynamic Desirable** initie la négociation.
- **Dynamic Auto avec Trunk :** Le port **Dynamic Auto** devient trunk car le port distant est déjà configuré en mode trunk.
- **Dynamic Auto avec Access :** Les deux ports restent en mode Access.

- **Dynamic Desirable avec Dynamic Auto :** Le port **Dynamic Desirable** initie la négociation, donc ils deviennent trunk.
- **Dynamic Desirable avec Dynamic Desirable :** Les deux ports deviennent trunk, car chacun initie la négociation.
- **Dynamic Desirable avec Trunk :** Le port **Dynamic Desirable** devient trunk car le port distant est en mode trunk.
- **Dynamic Desirable avec Access :** Les deux ports restent en mode Access.

- **Trunk avec Dynamic Auto :** Le port **Dynamic Auto** devient trunk, car le port distant est déjà en mode trunk.
- **Trunk avec Dynamic Desirable :** Le port **Dynamic Desirable** initie la négociation et les deux deviennent trunk.
- **Trunk avec Trunk :** Les deux ports sont configurés en trunk, donc ils restent en mode trunk.
- **Trunk avec Access :** Connectivité limitée, car les modes trunk et access ne sont pas compatibles (non recommandé).

- **Access avec Access :** Les deux ports restent en mode Access et communiquent uniquement sur un VLAN spécifique.

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


Voici les commandes pour configurer une interface en mode **trunk** avec l'encapsulation **dot1q** sur un commutateur Cisco :

### 10-Configuration d'une Interface Trunk avec Encapsulation dot1q

1. **Accéder au mode de configuration globale :**
   ```
   Switch# configure terminal
   ```

2. **Sélectionner l'interface à configurer (par exemple, FastEthernet 0/1) :**
   ```
   Switch(config)# interface f0/1
   ```

3. **Définir l'encapsulation trunk en **dot1q** (norme IEEE 802.1Q)** :
   ```
   Switch(config-if)# switchport trunk encapsulation dot1q
   ```

4. **Configurer le port en mode trunk :**
   ```
   Switch(config-if)# switchport mode trunk
   ```

5. **(Optionnel) Permettre uniquement certains VLANs sur le trunk (par exemple VLANs 10, 20) :**
   ```
   Switch(config-if)# switchport trunk allowed vlan 10,20
   ```

6. **Sauvegarder la configuration :**
   ```
   Switch(config-if)# end
   Switch# write memory
   ```

### Exemple Complet :
```bash
Switch# configure terminal
Switch(config)# interface f0/1
Switch(config-if)# switchport trunk encapsulation dot1q
Switch(config-if)# switchport mode trunk
Switch(config-if)# switchport trunk allowed vlan 10,20
Switch(config-if)# end
Switch# write memory
```

Cette configuration met l'interface FastEthernet 0/1 en mode **trunk** avec une encapsulation **dot1q**, permettant de transporter le trafic de plusieurs VLANs à travers un lien trunk.

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
