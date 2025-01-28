# L'Inter-VLAN (Routage Inter-VLAN)

L'inter-VLAN, ou routage inter-VLAN, permet aux périphériques situés dans des VLANs différents de communiquer entre eux. Les VLANs (Virtual Local Area Networks) segmentent logiquement un réseau en domaines de diffusion distincts, améliorant la sécurité et les performances réseau. Cependant, pour que des périphériques sur des VLANs différents puissent interagir, un routeur ou un switch de couche 3 est nécessaire.

---

## **Principaux Composants du Routage Inter-VLAN**

### 1. **Router-on-a-Stick**
- **Description** : Cette méthode repose sur un routeur utilisant une interface physique unique configurée en mode trunk. Cette interface gère le trafic de plusieurs VLANs via des sous-interfaces.
- **Fonctionnement** : Chaque sous-interface est associée à un VLAN spécifique, avec une adresse IP qui sert de passerelle par défaut pour ce VLAN.
- **Avantage** : Idéal pour des réseaux de petite taille ou à faible trafic.
- **Limitation** : La bande passante est limitée à l'interface unique du routeur.

### 2. **Switch de Couche 3**
- **Description** : Un switch de couche 3 combine les fonctions de routage et de commutation, permettant un routage inter-VLAN sans nécessiter un routeur externe.
- **Fonctionnement** : Le switch crée des interfaces virtuelles (SVI - Switch Virtual Interface) pour chaque VLAN. Ces SVIs agissent comme passerelles par défaut.
- **Avantage** : Performances accrues grâce au routage matériel et réduction de la latence.
- **Limitation** : Généralement plus coûteux que les solutions basées sur un routeur.

### 3. **Ports Trunks et Adressage**
- Les **ports trunks** sur les switches transportent le trafic de plusieurs VLANs simultanément.
- Chaque VLAN possède une adresse IP de passerelle par défaut configurée sur le routeur ou le switch de couche 3.

---

## **Étapes de Configuration : Router-on-a-Stick**

### Étape 1 : **Créer les VLANs sur le switch**
```bash
Switch(config)# vlan 10
Switch(config-vlan)# name Vente
Switch(config)# vlan 20
Switch(config-vlan)# name Marketing
```

### Étape 2 : **Configurer le port trunk sur le switch**
```bash
Switch(config)# interface gig0/1
Switch(config-if)# switchport mode trunk
```

### Étape 3 : **Configurer les sous-interfaces sur le routeur**
```bash
Router(config)# interface gig0/0.10
Router(config-subif)# encapsulation dot1Q 10
Router(config-subif)# ip address 192.168.10.1 255.255.255.0

Router(config)# interface gig0/0.20
Router(config-subif)# encapsulation dot1Q 20
Router(config-subif)# ip address 192.168.20.1 255.255.255.0
```

### Étape 4 : **Tester la Connectivité**
- Vérifiez que les périphériques dans chaque VLAN peuvent pinger leur passerelle par défaut.
- Assurez-vous que les périphériques dans des VLANs différents peuvent communiquer.

---

## **Résumé des Avantages**
- **Sécurité** : Les VLANs isolent les segments réseau sensibles.
- **Performance** : Réduction du trafic inutile grâce à la segmentation.
- **Flexibilité** : Le routage inter-VLAN permet une communication contrôlée entre segments.
