# 🕒 **VoIP : Configuration de la ToIP Cisco **  

La configuration de la ToIP Cisco que vous avez partagée inclut plusieurs étapes importantes pour le serveur DHCP, l'activation du service de téléphonie, et la configuration des téléphones sur le **Cisco Unified CME (CallManager Express)**. Voici une explication détaillée de chaque étape mentionnée, ainsi que quelques ajustements et explications pour une meilleure compréhension.

### 1. **Configuration du Serveur DHCP**
Le serveur DHCP (Dynamic Host Configuration Protocol) attribue des adresses IP aux téléphones IP Cisco automatiquement. Les commandes sont les suivantes :

- **Exclusion des adresses IP** : Vous excluez des adresses IP pour qu'elles ne soient pas attribuées aux téléphones IP, souvent celles que vous réservez pour des équipements spécifiques comme le serveur TFTP.
  ```bash
  Router(config)# ip dhcp excluded-address 192.168.1.1 192.168.1.20
  ```
  Ici, l'intervalle d'adresses IP exclut les adresses entre `192.168.1.1` et `192.168.1.20`.

- **Création du pool DHCP** : Vous créez un pool d'adresses IP qui seront attribuées aux appareils clients (téléphones IP, par exemple).
  ```bash
  Router(config)# ip dhcp pool VoIP_Pool
  Router(dhcp-config)# network 192.168.1.0 255.255.255.0
  Router(dhcp-config)# default-router 192.168.1.1
  Router(dhcp-config)# option 150 ip 192.168.1.50
  ```
  - `network 192.168.1.0 255.255.255.0` définit le réseau pour lequel DHCP attribuera les adresses IP.
  - `default-router 192.168.1.1` définit la passerelle par défaut, généralement le routeur ou le switch principal.
  - `option 150 ip 192.168.1.50` spécifie l'adresse IP du serveur TFTP, essentiel pour le provisioning des téléphones IP.

### 2. **Activation du Service de Téléphonie**
Ensuite, vous activez le service de téléphonie sur le **CME** :

- **Activation du service téléphonie** :
  ```bash
  Router(config)# telephony-service
  ```

- **Configurer les limites** : Vous définissez les capacités du CME, comme le nombre maximal de numéros dans l'annuaire et le nombre de téléphones IP.
  ```bash
  Router(config-telephony)# max-dn 200
  Router(config-telephony)# max-ephones 50
  ```
  - `max-dn` définit le nombre maximal d'entrées dans l'annuaire.
  - `max-ephones` définit le nombre maximum de téléphones IP que le CME peut gérer.

- **Configuration de l'adresse IP source** : Vous spécifiez l'adresse IP du CME, ainsi que le port utilisé pour la communication avec les téléphones.
  ```bash
  Router(config-telephony)# ip source-address 192.168.1.10 port 2000
  ```
  - `ip source-address 192.168.1.10` : Adresse IP du serveur CME.
  - `port 2000` : Numéro de port à utiliser pour la communication.

- **Assignation automatique des numéros d'extension** : Vous pouvez définir un intervalle pour l'assignation automatique des numéros d'extension aux téléphones.
  ```bash
  Router(config-telephony)# auto assign 1000 to 1050
  ```
  Cela attribuera automatiquement les numéros de téléphone de `1000` à `1050`.

### 3. **Configuration des Téléphones sur le CME**
Une fois que le service de téléphonie est activé, vous pouvez configurer les téléphones IP :

- **Définir une ligne (ephone-dn)** : Vous assignez un numéro d'ordre (DN) pour chaque ligne de téléphone.
  ```bash
  Router(config)# ephone-dn 1
  Router(config-ephone-dn)# number 1001
  ```
  Cela associe le numéro `1001` à l'extension de téléphone IP.

- **Associer un téléphone IP (ephone)** : Vous configurez chaque téléphone IP avec un numéro d'extension et d'autres paramètres nécessaires. Exemple pour un téléphone IP :
  ```bash
  Router(config)# ephone 1
  Router(config-ephone)# mac-address 00e0.4c02.9e8f
  Router(config-ephone)# button 1:1
  ```

### 4. **Vérification de la Configuration**
Pour vérifier que la configuration a été appliquée correctement, utilisez la commande suivante :

- **Vérification des téléphones enregistrés** :
  ```bash
  Router# show ephone
  ```
  Cela affiche les téléphones IP enregistrés et leur état.

---

Ces étapes vous permettent de configurer un serveur DHCP pour la téléphonie, d'activer le service de téléphonie sur un **CME** Cisco, de configurer les téléphones et d'effectuer des vérifications pour s'assurer que tout fonctionne correctement.