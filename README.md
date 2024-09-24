Voici un résumé des commandes de base pour la configuration des routeurs et des commutateurs Cisco, ainsi que quelques concepts clés comme DTP et VTP.

### Commandes de Base pour Routeurs et Commutateurs

#### Accès et Configuration
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

#### Configuration de l'Hôte
- **Renommer l'hôte :**
  ```bash
  Router(config)# hostname NouveauNom
  Switch(config)# hostname NouveauNom
  ```

#### Configuration des Interfaces
- **Interface FastEthernet :**
  ```bash
  Router(config)# interface FastEthernet numéro
  Router(config-if)# ip address @ masque_de_sous_réseau
  Router(config-if)# no shutdown
  Router(config-if)# exit
  ```
- **Interface Série :**
  ```bash
  Router(config)# interface serial numéro
  Router(config-if)# ip address @ masque
  Router(config-if)# clock rate nombre
  Router(config-if)# no shutdown
  Router(config-if)# exit
  ```
  
### Exemple de Configuration d'Interface Série
```bash
Router(config)# interface serial 0/0
Router(config-if)# ip address 192.168.1.1 255.255.255.0
Router(config-if)# clock rate 64000
Router(config-if)# no shutdown
Router(config-if)# exit
Router(config)# interface serial 0/0
Router(config-if)# ipv6 address 2001:cafe:2::2/64
Router(config-if)# no shutdown
Router(config-if)# exit
```

### Configuration des Mots de Passe
- **Accès par terminal :**
  ```bash
  Router(config)# line console 0
  Router(config-line)# password mot_de_passe
  Router(config-line)# login
  Router(config-line)# exit
  ```
- **Accès par Telnet :**
  ```bash
  Router(config)# line vty 0 4
  Router(config-line)# password mot_de_passe
  Router(config-line)# login
  Router(config-line)# exit
  ```
- **Accès SSH :**
  ```bash
  Router(config)# line vty 0 4
  Router(config-line)# transport input ssh
  Router(config-line)# login local
  Router(config-line)# exit
  ```
- **Mot de passe privilégié (crypté) :**
  ```bash
  Router(config)# enable secret mot_de_passe
  ```
- **Bannière :**
  ```bash
  Router(config)# banner motd # le message #
  ```
- **Crypter tous les mots de passe :**
  ```bash
  Router(config)# service password-encryption
  ```

### Configuration du Domaine
```bash
Router(config)# ip domain-name exemple.ma
```

### Génération de Clés RSA pour SSH
```bash
Router(config)# crypto key generate rsa
Router(config)# ip ssh version 2
```

### Création et Attribution de VLAN
- **Création de VLAN :**
  ```bash
  Switch# configure terminal
  Switch(config)# vlan vlan-id
  Switch(config-vlan)# name vlan-name
  Switch(config-vlan)# end
  ```
- **Attribution de Port à des VLAN :**
  ```bash
  Switch# configure terminal
  Switch(config)# interface interface-id
  Switch(config-if)# switchport mode access
  Switch(config-if)# switchport access vlan vlan-id
  Switch(config-if)# exit
  ```

