# 🔹 **Configuration des Listes de Contrôle d’Accès (ACL) sur un routeur Cisco**  

## 📌 **1. Introduction aux ACL**  
Les **ACL (Access Control Lists)** sont utilisées sur les routeurs Cisco pour filtrer le trafic réseau en fonction d’adresses IP, de protocoles, de ports, etc. Elles permettent de :
✅ **Restreindre l’accès** aux ressources réseau  
✅ **Améliorer la sécurité** en limitant certains types de trafic  
✅ **Optimiser la gestion du trafic**  

Il existe **deux types** d’ACL sur Cisco :  
- **ACL standard** (filtre uniquement sur les adresses IP source)  
- **ACL étendue** (filtre sur l’IP source/destination, protocole, port, etc.)  

---

## 📌 **2. ACL Standard**
**Les ACL standards filtrent uniquement sur l’adresse IP source.**  

### **🔹 Exemple de topologie**
Un réseau avec **trois sous-réseaux** :  
- **192.168.1.0/24** (réseau interne)  
- **192.168.2.0/24** (réseau invité)  
- **192.168.3.0/24** (réseau administratif)  

On veut **bloquer l’accès du réseau invité (192.168.2.0/24) au réseau administratif (192.168.3.0/24)**.

### **🔹 Étape 1 : Configurer une ACL Standard**
```bash
R1(config)# access-list 10 deny 192.168.2.0 0.0.0.255
R1(config)# access-list 10 permit any
```
📌 **Explication** :
- `access-list 10 deny 192.168.2.0 0.0.0.255` → Bloque tout le trafic du réseau 192.168.2.0  
- `access-list 10 permit any` → Autorise tout le reste du trafic  

---

### **🔹 Étape 2 : Appliquer l’ACL sur une Interface**
On applique cette ACL sur **l’interface connectée au réseau administratif** en entrée :
```bash
R1(config)# interface GigabitEthernet0/1
R1(config-if)# ip access-group 10 in
R1(config-if)# exit
```
📌 **Explication** :
- `ip access-group 10 in` → Applique l’ACL 10 sur le trafic **entrant**  

---

## 📌 **3. ACL Étendue**
**Les ACL étendues filtrent sur les adresses IP source/destination, protocole et ports.**  

### **🔹 Objectif**  
Bloquer **les connexions HTTP (port 80)** venant du réseau invité **192.168.2.0/24** vers le serveur Web **192.168.3.10**.

### **🔹 Étape 1 : Configurer une ACL Étendue**
```bash
R1(config)# access-list 100 deny tcp 192.168.2.0 0.0.0.255 host 192.168.3.10 eq 80
R1(config)# access-list 100 permit ip any any
```
📌 **Explication** :
- `deny tcp 192.168.2.0 0.0.0.255 host 192.168.3.10 eq 80` → Bloque HTTP (port 80) du réseau invité vers le serveur  
- `permit ip any any` → Autorise tout le reste du trafic  

---

### **🔹 Étape 2 : Appliquer l’ACL sur l’Interface**
Appliquer sur l’interface connectée au réseau invité **en sortie** :
```bash
R1(config)# interface GigabitEthernet0/2
R1(config-if)# ip access-group 100 out
R1(config-if)# exit
```
📌 **Explication** :
- `ip access-group 100 out` → Applique l’ACL 100 sur le trafic **sortant**  

---

## 📌 **4. ACL avec Numérotation Nominale**
Au lieu d’utiliser des numéros (`10`, `100`), on peut nommer les ACL.

### **🔹 Exemple : Bloquer le ping (ICMP) vers le réseau 192.168.3.0**
```bash
R1(config)# ip access-list extended BLOCK_PING
R1(config-ext-nacl)# deny icmp any 192.168.3.0 0.0.0.255
R1(config-ext-nacl)# permit ip any any
R1(config-ext-nacl)# exit
```
Appliquer sur l’interface connectée au réseau invité :
```bash
R1(config)# interface GigabitEthernet0/2
R1(config-if)# ip access-group BLOCK_PING in
```

---

## 📌 **5. Vérification et Dépannage**
### **🔹 Afficher les ACL configurées**
```bash
R1# show access-lists
```

### **🔹 Vérifier les ACL appliquées sur une interface**
```bash
R1# show ip interface GigabitEthernet0/1
```

### **🔹 Désactiver une ACL d’une interface**
```bash
R1(config)# interface GigabitEthernet0/1
R1(config-if)# no ip access-group 10 in
```

### **🔹 Supprimer une ACL**
```bash
R1(config)# no access-list 10
```

---

## ✅ **Résumé des Commandes Importantes**
| **Commande** | **Description** |
|--------------|-----------------|
| `access-list <numéro> permit/deny <protocole> <source> <masque> <destination> <port>` | Crée une ACL standard ou étendue |
| `ip access-group <numéro/nom> in/out` | Applique une ACL sur une interface |
| `ip access-list extended <nom>` | Crée une ACL nommée |
| `show access-lists` | Affiche les ACL configurées |
| `show ip interface <interface>` | Vérifie les ACL appliquées |
| `no access-list <numéro>` | Supprime une ACL |

---

## 🎯 **Conclusion**
- **ACL standard** : Filtre uniquement **sur l’IP source**  
- **ACL étendue** : Filtre **IP source/destination, protocole et ports**  
- **Appliquer les ACL au bon endroit** (trafic entrant ou sortant)  
- **Toujours inclure `permit ip any any` à la fin** pour éviter un blocage total  
