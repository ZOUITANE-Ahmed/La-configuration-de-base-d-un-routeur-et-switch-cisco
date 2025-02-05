# 🔹 **Configuration du NAT (Network Address Translation) sur un routeur Cisco**  

## 📌 **1. Introduction au NAT**  
Le **NAT (Network Address Translation)** est un processus qui permet de traduire les adresses IP privées d'un réseau interne vers une adresse IP publique pour l'accès à Internet. Ce processus est couramment utilisé pour :
- **Permettre à plusieurs hôtes** d'utiliser une seule adresse IP publique (NAT de type **overloading** ou PAT)
- **Améliorer la sécurité** en masquant les adresses internes
- **Simplifier la gestion des adresses IP publiques**

Les types de NAT incluent :
1. **Static NAT** : Une adresse IP privée est mappée à une adresse IP publique.
2. **Dynamic NAT** : Une adresse IP privée est mappée à une adresse IP publique disponible dans un pool.
3. **PAT (Port Address Translation)** : Plusieurs adresses IP privées peuvent partager une seule adresse IP publique avec des ports différents.

---

## 📌 **2. Configuration du Static NAT**  
Le **Static NAT** permet de traduire une adresse IP privée spécifique vers une adresse IP publique spécifique.

### **🔹 Exemple de topologie**
- **192.168.1.10** (hôte interne)
- **200.200.200.10** (adresse IP publique)

### **🔹 Étape 1 : Configurer le Static NAT**
```bash
R1(config)# ip nat inside source static 192.168.1.10 200.200.200.10
```
📌 **Explication** : Cette commande configure le **Static NAT**, mappant l'adresse privée **192.168.1.10** à l'adresse publique **200.200.200.10**.

### **🔹 Étape 2 : Définir les Interfaces**
Ensuite, il faut définir les interfaces internes et externes sur le routeur.

```bash
R1(config)# interface GigabitEthernet0/0
R1(config-if)# ip nat inside  # Interface interne
R1(config-if)# exit

R1(config)# interface GigabitEthernet0/1
R1(config-if)# ip nat outside  # Interface externe
R1(config-if)# exit
```

---

## 📌 **3. Configuration du Dynamic NAT avec un Pool d’IP**  
Le **Dynamic NAT** attribue une adresse IP publique dynamique à partir d'un pool d'adresses IP disponibles.

### **🔹 Exemple de topologie**
- Réseau interne : **192.168.1.0/24**
- Pool d’adresses IP publiques : **200.200.200.10 - 200.200.200.20**

### **🔹 Étape 1 : Créer un Pool d’IP Publics**
```bash
R1(config)# ip nat pool PUBLIC_POOL 200.200.200.10 200.200.200.20 netmask 255.255.255.0
```
📌 **Explication** : Crée un pool d’adresses IP publiques allant de **200.200.200.10** à **200.200.200.20**.

### **🔹 Étape 2 : Configurer un Access List pour le NAT**
L'ACL définit quelles adresses privées peuvent être traduites par NAT.

```bash
R1(config)# access-list 1 permit 192.168.1.0 0.0.0.255
```
📌 **Explication** : Cette ACL permet aux adresses IP du réseau **192.168.1.0/24** d'être traduites par NAT.

### **🔹 Étape 3 : Appliquer le Dynamic NAT**
```bash
R1(config)# ip nat inside source list 1 pool PUBLIC_POOL
```
📌 **Explication** : Applique l'ACL (liste 1) et le pool d'adresses IP publiques pour effectuer la traduction d'adresses IP dynamiques.

### **🔹 Étape 4 : Définir les Interfaces**
Définir l'interface interne et externe :

```bash
R1(config)# interface GigabitEthernet0/0
R1(config-if)# ip nat inside
R1(config-if)# exit

R1(config)# interface GigabitEthernet0/1
R1(config-if)# ip nat outside
R1(config-if)# exit
```

---

## 📌 **4. Configuration du PAT (Port Address Translation)**  
Le **PAT** permet de traduire plusieurs adresses IP privées vers une seule adresse IP publique, en utilisant des numéros de port différents pour chaque session.

### **🔹 Exemple de topologie**
- **Réseau interne** : **192.168.1.0/24**
- **Adresse IP publique** : **200.200.200.10**

### **🔹 Étape 1 : Configurer le PAT**
```bash
R1(config)# ip nat inside source list 1 interface GigabitEthernet0/1 overload
```
📌 **Explication** :  
- `source list 1` → Utilise l’ACL 1 pour le NAT.  
- `interface GigabitEthernet0/1` → Utilise l'adresse IP de l'interface externe (200.200.200.10).  
- `overload` → Permet d'utiliser une seule adresse IP publique pour plusieurs hôtes internes en distinguant les sessions par les numéros de port.

### **🔹 Étape 2 : Définir les Interfaces**
Définir l'interface interne et externe :

```bash
R1(config)# interface GigabitEthernet0/0
R1(config-if)# ip nat inside
R1(config-if)# exit

R1(config)# interface GigabitEthernet0/1
R1(config-if)# ip nat outside
R1(config-if)# exit
```

---

## 📌 **5. Vérification et Dépannage du NAT**  
### **🔹 Vérifier le NAT**
Pour vérifier l'état du NAT, utilise la commande suivante :
```bash
R1# show ip nat translations
```
Cela affiche les traductions d’adresses IP en cours.

### **🔹 Vérifier les interfaces NAT**
```bash
R1# show ip nat statistics
```
Cela montre les statistiques de NAT, y compris le nombre de traductions effectuées.

---

## ✅ **Résumé des Commandes Importantes**
| **Commande** | **Description** |
|--------------|-----------------|
| `ip nat inside source static <ip_privee> <ip_publique>` | Configure un NAT statique |
| `ip nat pool <nom_pool> <ip_debut> <ip_fin> netmask <masque>` | Crée un pool d'adresses IP publiques pour le NAT dynamique |
| `ip nat inside source list <num_acl> pool <nom_pool>` | Applique le NAT dynamique avec un pool d'adresses IP |
| `ip nat inside source list <num_acl> interface <interface> overload` | Configure le PAT avec l'interface externe et l'option de surcharge |
| `show ip nat translations` | Affiche les traductions NAT en cours |
| `show ip nat statistics` | Affiche les statistiques NAT |

---

## 🎯 **Conclusion**
- Le **Static NAT** associe une adresse IP privée à une adresse IP publique de manière **fixe**.
- Le **Dynamic NAT** utilise un **pool d'adresses** publiques pour traduire les adresses privées.
- Le **PAT** permet à **plusieurs hôtes internes** de partager une seule adresse IP publique.