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
