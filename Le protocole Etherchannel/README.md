### **EtherChannel : Définition et Fonctionnement**  

**EtherChannel** est une technologie développée par **Cisco** qui permet d’agréger plusieurs liens physiques Ethernet en un seul lien logique. Cette agrégation améliore la **bande passante**, la **redondance** et l’**équilibrage de charge** entre les interfaces d’un switch.

---

### **1. Avantages de l’EtherChannel**  
✅ **Augmentation de la bande passante** : En combinant plusieurs liens physiques, on obtient une capacité supérieure. Par exemple, avec 4 liens de 1 Gbps, on peut obtenir une bande passante totale de 4 Gbps.  
✅ **Tolérance aux pannes** : Si un lien tombe, les autres continuent de fonctionner sans interruption.  
✅ **Équilibrage de charge** : La répartition du trafic se fait en fonction des paramètres de hashage (MAC source/destination, IP source/destination, ports TCP/UDP, etc.).  
✅ **Configuration simplifiée** : Une seule interface logique (port-channel) est configurée, ce qui facilite la gestion.  

---

### **2. Modes de Configuration d’EtherChannel**  

Il existe **trois** modes d’agrégation des liens :  

#### **A. Mode statique** (ON)  
- Le mode **"on"** force l’activation de l’EtherChannel sans protocole de négociation.  
- ⚠️ Doit être configuré **manuellement** sur les deux switches.  

#### **B. Mode dynamique avec PAgP (Port Aggregation Protocol)**  
- Protocole **Cisco propriétaire** qui permet la négociation automatique des liens.  
- Deux modes :  
  - **Desirable** : Active l’agrégation si l’autre interface est en **desirable** ou **auto**.  
  - **Auto** : Attend que l’autre interface soit en **desirable** pour établir l’agrégation.  

#### **C. Mode dynamique avec LACP (Link Aggregation Control Protocol)**  
- **Protocole standard IEEE 802.3ad** (interopérable entre différents fabricants).  
- Deux modes :  
  - **Active** : Envoie des paquets LACP pour former un agrégat.  
  - **Passive** : Accepte l’agrégation seulement si l’autre côté est en **active**.  

| Mode | Fonctionne avec | Propriétaire | Négociation automatique |
|------|---------------|--------------|------------------------|
| **ON** | Aucun protocole | Non | ❌ |
| **PAgP (Auto)** | Cisco | Oui | ✅ (si autre en Desirable) |
| **PAgP (Desirable)** | Cisco | Oui | ✅ |
| **LACP (Passive)** | Tous (IEEE 802.3ad) | Non | ✅ (si autre en Active) |
| **LACP (Active)** | Tous (IEEE 802.3ad) | Non | ✅ |

---

### **3. Configuration de l’EtherChannel sur un switch Cisco**  

#### **A. Configuration en mode statique**
```bash
Switch(config)# interface range GigabitEthernet 0/1 - 2
Switch(config-if-range)# channel-group 1 mode on
Switch(config-if-range)# exit
Switch(config)# interface Port-channel 1
Switch(config-if)# switchport mode trunk  # Si VLANs sont utilisés
```

#### **B. Configuration avec PAgP**
```bash
Switch(config)# interface range GigabitEthernet 0/1 - 2
Switch(config-if-range)# channel-group 1 mode desirable
```

#### **C. Configuration avec LACP**
```bash
Switch(config)# interface range GigabitEthernet 0/1 - 2
Switch(config-if-range)# channel-group 1 mode active
```

---

### **4. Vérification de l’état de l’EtherChannel**  
📌 **Vérifier les interfaces dans l’EtherChannel**  
```bash
Switch# show etherchannel summary
```
📌 **Voir les ports membres et leur état**  
```bash
Switch# show interfaces port-channel 1
```

---

### **5. Bonnes Pratiques et Précautions**  
⚠️ **Les interfaces doivent être compatibles** : Même vitesse, même duplex, même type de port (access/trunk).  
⚠️ **Évitez le mode "on"** sans coordination avec l’autre switch, car cela peut provoquer des boucles réseau.  
⚠️ **Vérifiez la cohérence des configurations** sur les deux équipements pour éviter les erreurs de négociation.  

---

### **6. Scénarios d’Utilisation**  
🔹 **Entre deux switches pour améliorer la liaison montante (uplink)**  
🔹 **Entre un serveur et un switch pour un meilleur débit**  
🔹 **Entre un routeur et un switch pour répartir le trafic réseau**  

EtherChannel est donc une solution efficace pour optimiser la bande passante et la redondance dans un réseau, surtout en entreprise. 🚀