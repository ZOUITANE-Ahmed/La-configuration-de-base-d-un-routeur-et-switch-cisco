Les protocoles **CDP (Cisco Discovery Protocol)** et **LLDP (Link Layer Discovery Protocol)** sont des protocoles de découverte de voisins utilisés dans les réseaux pour collecter des informations sur les périphériques connectés.  

---

### 🔹 **CDP (Cisco Discovery Protocol)**
📌 **Propriétaire** : Cisco  
📌 **Norme** : Propriétaire (spécifique aux équipements Cisco)  
📌 **Niveau OSI** : Couche 2 (liaison de données)  
📌 **Fonctionnement** :
- CDP permet aux périphériques Cisco d'échanger des informations sur leurs interfaces connectées.  
- Il fonctionne en envoyant périodiquement des messages **CDP advertisements** contenant des détails sur le périphérique (nom, IP, plateforme, version du logiciel, etc.).  
- Les équipements Cisco peuvent utiliser CDP pour découvrir leurs voisins et faciliter la configuration et le dépannage.  

📌 **Commandes principales** (sur Cisco IOS) :
```bash
show cdp neighbors            # Afficher les voisins CDP détectés
show cdp neighbors detail     # Afficher des détails supplémentaires (IP, modèle, etc.)
show cdp interface            # Vérifier les interfaces activées pour CDP
no cdp enable                 # Désactiver CDP sur une interface
```
📌 **Limitation** :
- CDP ne fonctionne qu’avec les équipements Cisco.  
- Il peut représenter un risque de sécurité s’il est activé sur des interfaces accessibles au public.

---

### 🔹 **LLDP (Link Layer Discovery Protocol)**
📌 **Propriétaire** : Standard ouvert  
📌 **Norme** : IEEE 802.1AB  
📌 **Niveau OSI** : Couche 2 (liaison de données)  
📌 **Fonctionnement** :
- LLDP est un protocole standard permettant l’échange d’informations entre **des équipements réseau de différents fabricants**.  
- Il envoie des **LLDP advertisements** périodiques contenant des informations sur le périphérique, son interface et ses capacités.  
- Les équipements de marques différentes (Cisco, HP, Juniper, etc.) peuvent utiliser LLDP pour se découvrir mutuellement.  

📌 **Commandes principales** (sur Cisco IOS) :
```bash
show lldp neighbors           # Afficher les voisins LLDP détectés
show lldp neighbors detail    # Afficher des détails supplémentaires
lldp run                      # Activer LLDP globalement
no lldp enable                # Désactiver LLDP sur une interface
```
📌 **Avantages** :
- **Interopérabilité** : Fonctionne avec des équipements de différents fabricants.  
- **Meilleure visibilité** dans les environnements hétérogènes.  

📌 **Limitation** :
- LLDP n'est pas activé par défaut sur les équipements Cisco (contrairement à CDP).  
- Moins de détails spécifiques aux équipements Cisco par rapport à CDP.

---

### 📌 **CDP vs LLDP : Tableau Comparatif**  

| **Critère**       | **CDP** 🏷️ | **LLDP** 🌎 |
|-----------------|------------|------------|
| **Type** | Propriétaire (Cisco) | Standard (IEEE 802.1AB) |
| **Interopérabilité** | Cisco uniquement | Compatible avec tous les fabricants |
| **Activé par défaut ?** | Oui (Cisco) | Non (Cisco) |
| **Détails échangés** | Nom, modèle, IP, version IOS, VLAN, etc. | Nom, modèle, IP, port, VLAN, etc. |
| **Sécurité** | Peut poser un risque s’il est activé sur des ports accessibles | Plus sécurisé car utilisé dans des environnements contrôlés |

### **📌 Conclusion**
- **Si votre réseau est 100% Cisco** ➝ **CDP** est plus détaillé et pratique.  
- **Si vous avez un réseau hétérogène (Cisco, Juniper, HP, etc.)** ➝ **LLDP** est préférable.  
- Pour la **sécurité**, évitez d’activer CDP ou LLDP sur des interfaces exposées au public.

---

Voici la configuration des protocoles **CDP** et **LLDP** sur un **switch Cisco**.

---

## 📌 **Configuration de CDP (Cisco Discovery Protocol)**

CDP est activé par défaut sur les équipements Cisco.  
Si ce n'est pas le cas, voici comment l'activer :

### ✅ **Activation de CDP**
```bash
Switch(config)# cdp run   # Active CDP globalement
Switch(config-if)# cdp enable   # Active CDP sur une interface spécifique
```

### ❌ **Désactivation de CDP**
```bash
Switch(config)# no cdp run   # Désactive CDP globalement
Switch(config-if)# no cdp enable   # Désactive CDP sur une interface spécifique
```

### 🔎 **Vérification de l'état de CDP**
```bash
Switch# show cdp   # Vérifie si CDP est actif
Switch# show cdp neighbors   # Affiche les voisins CDP connectés
Switch# show cdp neighbors detail   # Affiche des détails sur les voisins
Switch# show cdp interface   # Vérifie si CDP est activé sur une interface spécifique
```

---

## 📌 **Configuration de LLDP (Link Layer Discovery Protocol)**

LLDP n'est **pas activé** par défaut sur les équipements Cisco.  
Voici comment l’activer :

### ✅ **Activation de LLDP**
```bash
Switch(config)# lldp run   # Active LLDP globalement
Switch(config-if)# lldp transmit   # Active l’envoi des annonces LLDP sur une interface
Switch(config-if)# lldp receive   # Active la réception des annonces LLDP sur une interface
```

### ❌ **Désactivation de LLDP**
```bash
Switch(config)# no lldp run   # Désactive LLDP globalement
Switch(config-if)# no lldp transmit   # Désactive l’envoi des annonces LLDP sur une interface
Switch(config-if)# no lldp receive   # Désactive la réception des annonces LLDP sur une interface
```

### 🔎 **Vérification de l'état de LLDP**
```bash
Switch# show lldp   # Vérifie si LLDP est actif
Switch# show lldp neighbors   # Affiche les voisins LLDP connectés
Switch# show lldp neighbors detail   # Affiche des détails sur les voisins
Switch# show lldp interface   # Vérifie si LLDP est activé sur une interface spécifique
```

---

## 🎯 **Cas d'usage**
1. **Environnement 100% Cisco** ➝ **Utiliser CDP**  
2. **Environnement multi-constructeurs** ➝ **Utiliser LLDP**  
3. **Besoin d'un maximum d'informations sur les voisins** ➝ **Activer les deux (si compatibilité requise)**  

