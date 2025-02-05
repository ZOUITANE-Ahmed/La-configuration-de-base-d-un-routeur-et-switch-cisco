# 🔹 **Configuration VPN sur un routeur Cisco**  

Un **VPN (Virtual Private Network)** permet de sécuriser les connexions entre des réseaux distants en chiffrant les données. Il existe plusieurs types de VPN sur un routeur Cisco, notamment **VPN Site-to-Site (IPsec)** et **VPN à accès distant (Remote Access VPN - SSL ou IPsec)**.  

---

## 📌 **1. Configuration d'un VPN Site-to-Site IPsec**  
Un **VPN Site-to-Site IPsec** permet de sécuriser la communication entre **deux sites distants** via Internet.  

### **🔹 Topologie**
- **Routeur 1 (R1)** (Site A)  
  - LAN : **192.168.1.0/24**  
  - Interface WAN : **200.200.200.1**  
- **Routeur 2 (R2)** (Site B)  
  - LAN : **192.168.2.0/24**  
  - Interface WAN : **200.200.200.2**  

---

### **🔹 Étape 1 : Configurer les listes d’accès pour identifier le trafic chiffré**
Sur **R1** :
```bash
R1(config)# access-list 100 permit ip 192.168.1.0 0.0.0.255 192.168.2.0 0.0.0.255
```
Sur **R2** :
```bash
R2(config)# access-list 100 permit ip 192.168.2.0 0.0.0.255 192.168.1.0 0.0.0.255
```
📌 **Explication** : Cette ACL définit le trafic qui doit être protégé par le VPN (entre les LANs des deux sites).  

---

### **🔹 Étape 2 : Configurer l’ISAKMP (Phase 1)**
Sur **R1** :
```bash
R1(config)# crypto isakmp policy 10
R1(config-isakmp)# encryption aes 256
R1(config-isakmp)# hash sha256
R1(config-isakmp)# authentication pre-share
R1(config-isakmp)# group 14
R1(config-isakmp)# lifetime 86400
```
Sur **R2** :
```bash
R2(config)# crypto isakmp policy 10
R2(config-isakmp)# encryption aes 256
R2(config-isakmp)# hash sha256
R2(config-isakmp)# authentication pre-share
R2(config-isakmp)# group 14
R2(config-isakmp)# lifetime 86400
```
📌 **Explication** :  
- AES 256 → Algorithme de chiffrement  
- SHA256 → Algorithme de hachage  
- Authentification par clé partagée (pre-share)  
- Groupe Diffie-Hellman 14 pour l’échange de clés  

---

### **🔹 Étape 3 : Définir la clé pré-partagée**
Sur **R1** :
```bash
R1(config)# crypto isakmp key VPNSECRETKEY address 200.200.200.2
```
Sur **R2** :
```bash
R2(config)# crypto isakmp key VPNSECRETKEY address 200.200.200.1
```
📌 **Explication** : Définit une clé partagée entre les deux routeurs pour l’authentification.  

---

### **🔹 Étape 4 : Configurer l’IPsec (Phase 2)**
Sur **R1** :
```bash
R1(config)# crypto ipsec transform-set MYSET esp-aes 256 esp-sha256-hmac
```
Sur **R2** :
```bash
R2(config)# crypto ipsec transform-set MYSET esp-aes 256 esp-sha256-hmac
```
📌 **Explication** : Définit le chiffrement AES 256 et l’intégrité SHA256 pour les paquets IPsec.  

---

### **🔹 Étape 5 : Créer un profil IPsec**
Sur **R1** :
```bash
R1(config)# crypto map VPNMAP 10 ipsec-isakmp
R1(config-crypto-map)# set peer 200.200.200.2
R1(config-crypto-map)# set transform-set MYSET
R1(config-crypto-map)# match address 100
```
Sur **R2** :
```bash
R2(config)# crypto map VPNMAP 10 ipsec-isakmp
R2(config-crypto-map)# set peer 200.200.200.1
R2(config-crypto-map)# set transform-set MYSET
R2(config-crypto-map)# match address 100
```
📌 **Explication** :  
- Définit un **crypto map** qui associe l’IPsec aux ACL définies plus tôt.  
- Associe le **transform-set** pour sécuriser le trafic.  

---

### **🔹 Étape 6 : Appliquer la configuration sur l’interface WAN**
Sur **R1** :
```bash
R1(config)# interface GigabitEthernet0/0
R1(config-if)# crypto map VPNMAP
```
Sur **R2** :
```bash
R2(config)# interface GigabitEthernet0/0
R2(config-if)# crypto map VPNMAP
```
📌 **Explication** : Applique la configuration VPN sur l’interface publique.  

---

### **🔹 Vérification du VPN**
#### 🔹 Vérifier l’établissement de la session ISAKMP (Phase 1)
```bash
show crypto isakmp sa
```
#### 🔹 Vérifier les tunnels IPsec (Phase 2)
```bash
show crypto ipsec sa
```
#### 🔹 Vérifier les paquets chiffrés
```bash
show crypto engine connections active
```

---

## 📌 **2. Configuration d’un VPN à accès distant (Remote Access VPN)**
Un **VPN à accès distant** permet aux utilisateurs externes (télétravail) de se connecter au réseau interne de l’entreprise de manière sécurisée.

### 🔹 **Étape 1 : Créer un pool d’adresses VPN**
```bash
R1(config)# ip local pool VPNPOOL 192.168.10.10 192.168.10.100
```
📌 **Explication** : Définit une plage d’IP pour les utilisateurs VPN.  

### 🔹 **Étape 2 : Activer l’authentification locale**
```bash
R1(config)# username vpnuser password cisco123
```
📌 **Explication** : Crée un compte utilisateur pour l’accès VPN.  

### 🔹 **Étape 3 : Configurer l’ISAKMP et IPsec**
```bash
R1(config)# crypto isakmp policy 10
R1(config-isakmp)# encryption aes 256
R1(config-isakmp)# hash sha256
R1(config-isakmp)# authentication pre-share
R1(config-isakmp)# group 14
```

### 🔹 **Étape 4 : Configurer le serveur VPN**
```bash
R1(config)# crypto isakmp client configuration group VPNCLIENT
R1(config-isakmp-group)# key VPNKEY
R1(config-isakmp-group)# pool VPNPOOL
```
📌 **Explication** : Définit la clé VPN et le pool d’adresses pour les clients.  

---

## ✅ **Résumé des Commandes Importantes**
| **Commande** | **Description** |
|--------------|-----------------|
| `crypto isakmp policy 10` | Définit la politique ISAKMP (Phase 1) |
| `crypto ipsec transform-set MYSET esp-aes 256 esp-sha256-hmac` | Définit la transformation IPsec |
| `crypto map VPNMAP 10 ipsec-isakmp` | Associe l’IPsec à une adresse distante |
| `interface GigabitEthernet0/0`<br>`crypto map VPNMAP` | Applique le VPN sur l’interface WAN |
| `show crypto isakmp sa` | Vérifie la connexion ISAKMP (Phase 1) |
| `show crypto ipsec sa` | Vérifie l’établissement du tunnel VPN |

---

## 🎯 **Conclusion**
- **VPN Site-to-Site** sécurise la communication entre **deux réseaux distants**.  
- **VPN Remote Access** permet aux utilisateurs externes de se connecter en toute sécurité.  
- **L’IPsec assure** le chiffrement et l’intégrité des données.  