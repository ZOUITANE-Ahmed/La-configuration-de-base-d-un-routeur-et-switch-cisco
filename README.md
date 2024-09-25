Voici un résumé des commandes de base pour la configuration des routeurs et des commutateurs Cisco, ainsi que quelques concepts clés comme DTP et VTP.

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
- **3-a-ipv4 : Interface FastEthernet :**
  ```bash
  Router(config)# interface FastEthernet numéro
  Router(config-if)# ip address @ masque_de_sous_réseau
  Router(config-if)# no shutdown
  Router(config-if)# exit
  ```
  - **3-b-ipv6 : Interface FastEthernet :**
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

