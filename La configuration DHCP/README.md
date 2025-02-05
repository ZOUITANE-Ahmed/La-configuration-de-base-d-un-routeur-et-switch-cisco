La configuration d'un serveur **DHCP** sur un routeur **Cisco** permet d'attribuer automatiquement des adresses IP aux clients du réseau. Voici les étapes générales pour configurer un serveur **DHCP** sur un routeur **Cisco**.

---

### 📌 **1. Accéder au mode de configuration**
Tout d'abord, connectez-vous au routeur via **console, SSH ou Telnet**, puis passez en mode de configuration globale :

```bash
 Router>enable
 Router#configure terminal
```

---

### 📌 **2. Créer un pool DHCP**
Un pool DHCP définit une plage d'adresses IP à attribuer aux clients :

```bash
 Router(config)#ip dhcp pool NOM_DU_POOL
 Router(dhcp-config)#network 192.168.1.0 255.255.255.0  # Plage d'adresses IP
 Router(dhcp-config)#default-router 192.168.1.1         # Passerelle par défaut
 Router(dhcp-config)#dns-server 8.8.8.8 8.8.4.4         # Serveurs DNS
 Router(dhcp-config)#lease 7                            # Durée du bail en jours (ici, 7 jours)
```

---

### 📌 **3. Exclure certaines adresses IP**
Si certaines adresses doivent être **réservées** (pour des serveurs, imprimantes, etc.), on peut les exclure du pool :

```bash
 Router(config)#ip dhcp excluded-address 192.168.1.1 192.168.1.10
```
Ici, les adresses **192.168.1.1 à 192.168.1.10** ne seront **pas attribuées** par le DHCP.

---

### 📌 **4. Activer le service DHCP**
Le service DHCP est activé par défaut, mais au cas où il serait désactivé, utilisez la commande :

```bash
 Router(dhcp-config)#service dhcp
```

---

### 📌 **5. Vérification et dépannage**
Pour vérifier que le DHCP fonctionne correctement :

✅ **Afficher les baux DHCP attribués :**
```bash
 Router#show ip dhcp binding
```

✅ **Afficher l'état du serveur DHCP :**
```bash
 Router#show ip dhcp server statistics
```

✅ **Vérifier les logs en cas de problème :**
```bash
 Router#debug ip dhcp server events
```

---

### 📌 **6. Sauvegarder la configuration**
Une fois la configuration terminée et testée, enregistrez-la pour éviter de la perdre après un redémarrage :

```bash
 Router#write memory
```
ou
```bash
 Router#copy running-config startup-config
```

---

### 🔥 **Exemple Complet**
Si vous voulez un exemple concret, voici une configuration prête à l'emploi :

```bash
 Router>enable
 Router(config)#configure terminal

 Router(config)#ip dhcp excluded-address 192.168.1.1 192.168.1.10
 Router(config)#ip dhcp pool LAN_POOL
 Router(dhcp-config)#network 192.168.1.0 255.255.255.0
 Router(dhcp-config)#default-router 192.168.1.1
 Router(dhcp-config)#dns-server 8.8.8.8 8.8.4.4
 Router(dhcp-config)#lease 7

 Router(dhcp-config)#service dhcp

 Router(config)#exit
 Router(config)#write memory
```

---

### 🎯 **Conclusion**
Cette configuration permet à un routeur **Cisco** de distribuer dynamiquement des adresses IP aux clients du réseau. Assurez-vous que l'interface qui connecte les clients est bien activée avec **`ip address`** et que les clients sont bien configurés en **DHCP**.

Si tu as des questions ou un scénario spécifique en tête, fais-moi signe ! 🚀