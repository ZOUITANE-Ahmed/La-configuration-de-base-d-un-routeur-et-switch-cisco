La **sécurité des ports** (Port Security) est une fonctionnalité des commutateurs réseau qui permet de **restreindre l'accès aux ports physiques** en contrôlant les adresses MAC des appareils connectés. Elle est principalement utilisée pour **prévenir les attaques réseau** et éviter les connexions non autorisées.  

### 🔹 **Principes de la sécurité des ports**  
Port Security fonctionne en **limitant le nombre d'adresses MAC** autorisées sur un port donné et en définissant des actions spécifiques lorsque des violations sont détectées.  

### 🔹 **Modes de fonctionnement**  
1. **Dynamic (Dynamique)** – Apprend automatiquement les adresses MAC et les oublie en cas de déconnexion.  
2. **Sticky (Collant)** – Apprend et enregistre les adresses MAC dans la configuration en cours (et peut être sauvegardé dans la configuration du commutateur).  
3. **Static (Statique)** – L'administrateur configure manuellement les adresses MAC autorisées sur un port.  

### 🔹 **Actions en cas de violation**  
Si une adresse MAC non autorisée tente d'accéder au port, plusieurs actions peuvent être configurées :  
- **Protect (Protection)** : Ignore les adresses MAC non autorisées sans couper le port.  
- **Restrict (Restriction)** : Bloque les adresses MAC non autorisées et génère un message d'alerte.  
- **Shutdown (Arrêt)** : Désactive le port et le met en état *err-disabled* (nécessite une intervention manuelle pour le réactiver).  

### 🔹 **Configuration sur un switch Cisco**  
```bash
Switch(config)# interface FastEthernet 0/1
Switch(config-if)# switchport mode access
Switch(config-if)# switchport port-security
Switch(config-if)# switchport port-security maximum 2
Switch(config-if)# switchport port-security violation shutdown
Switch(config-if)# switchport port-security mac-address sticky
Switch(config-if)# end
```
📌 **Explication** :  
- Active Port Security sur le port `FastEthernet 0/1`.  
- Limite à **2 adresses MAC** autorisées.  
- En cas de violation, le port est désactivé.  
- Utilise le mode **sticky**, donc les adresses MAC apprises sont enregistrées.  

### 🔹 **Avantages de la sécurité des ports**  
✅ Protège contre les attaques de type **MAC flooding**.  
✅ Empêche la connexion de **périphériques non autorisés**.  
✅ Réduit les risques liés aux **attaques internes**.  

### 🔹 **Limitations**  
⚠️ Ne protège pas contre **les attaques de spoofing** avancées (ex : modification d'adresse MAC).  
⚠️ Peut générer des **interruptions réseau** si mal configuré.  

🔹 **Conclusion**  
La sécurité des ports est un **outil efficace** pour renforcer la sécurité d’un réseau local, mais elle doit être utilisée avec d’autres **mesures de sécurité** comme **802.1X**, les VLANs sécurisés et la segmentation réseau.  
