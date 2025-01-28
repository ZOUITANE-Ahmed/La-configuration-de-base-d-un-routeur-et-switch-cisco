Le **protocole STP** (Spanning Tree Protocol) est un protocole réseau utilisé pour éviter les boucles dans les réseaux commutés (switchés). Ces boucles peuvent se produire lorsqu'il existe des chemins redondants entre des commutateurs, ce qui est souvent une pratique courante pour assurer la tolérance aux pannes. Cependant, elles peuvent entraîner des problèmes comme des tempêtes de broadcast, des duplications de paquets, et une surcharge du réseau.

### Fonctionnement du STP :
STP est défini dans la norme **IEEE 802.1D** et fonctionne en créant une **arborescence logique sans boucle** (Spanning Tree) pour désactiver les chemins redondants tout en permettant leur réactivation en cas de défaillance.

#### Étapes principales :
1. **Élection du Root Bridge :**
   - Le commutateur avec l'identifiant Bridge ID (BID) le plus bas devient le **Root Bridge**.
   - Le BID est une combinaison de deux valeurs : une priorité (par défaut 32768) et l'adresse MAC du commutateur.

2. **Détermination des Root Ports :**
   - Chaque commutateur identifie le port qui mène au Root Bridge avec le chemin de coût le plus bas. Ce port est appelé **Root Port**.

3. **Élection des Designated Ports :**
   - Parmi les liens connectés à chaque segment réseau, un seul port est élu comme **Designated Port** (celui avec le coût le plus faible vers le Root Bridge).

4. **Blocage des ports redondants :**
   - Les ports restants sont mis en état de **blocage** pour éviter les boucles.

#### États des ports dans STP :
- **Blocking** : Ne transmet ni ne reçoit de données, écoute uniquement les BPDU.
- **Listening** : Écoute les BPDU pour déterminer si une boucle existe.
- **Learning** : Apprend les adresses MAC mais ne transmet pas encore de trafic.
- **Forwarding** : Transmet et reçoit les données normalement.
- **Disabled** : État administrativement désactivé.

#### Les BPDU (Bridge Protocol Data Units) :
- STP utilise des messages appelés **BPDU** pour échanger des informations entre les commutateurs.
- Ils contiennent les informations nécessaires à l'élection du Root Bridge et à la détermination des rôles des ports.

---

### Types de STP :
1. **STP classique (IEEE 802.1D)** : La version originale, mais lente à converger (~30 à 50 secondes).
2. **RSTP (Rapid Spanning Tree Protocol, IEEE 802.1w)** : Une version améliorée qui réduit le temps de convergence à quelques secondes.
3. **MSTP (Multiple Spanning Tree Protocol, IEEE 802.1s)** : Permet la gestion de plusieurs instances d'arborescence pour différents VLAN.
4. **PVST+/RPVST+** : Protocoles propriétaires de Cisco qui créent une instance STP par VLAN.

---

### Avantages :
- Évite les boucles dans les réseaux commutés.
- Permet une redondance avec des chemins de secours.
- Maintient la stabilité du réseau.

### Inconvénients :
- STP classique est lent à converger.
- La configuration manuelle peut être complexe dans les grands réseaux.
- RSTP ou MSTP sont préférables pour de meilleures performances.


### 1. **Activer le protocole STP** (activé par défaut)
Par défaut, STP est activé sur tous les switches Cisco. Si vous voulez vérifier son état :

```bash
Switch> enable
Switch# show spanning-tree
```

Si STP est désactivé, vous pouvez l'activer sur un VLAN spécifique ou globalement :

```bash
Switch(config)# spanning-tree vlan [vlan-id]
```

---

### 2. **Configuration de la priorité pour le Root Bridge**
Le commutateur avec la plus **faible priorité** devient le **Root Bridge**. Vous pouvez forcer un commutateur à devenir Root Bridge en ajustant sa priorité :

```bash
Switch(config)# spanning-tree vlan [vlan-id] priority [valeur]
```

> ⚠️ La priorité par défaut est **32768**, et elle doit être un multiple de **4096**.

Pour simplifier, Cisco propose aussi des commandes prédéfinies :

- **Forcer comme Root Primary** (priorité la plus basse) :
  ```bash
  Switch(config)# spanning-tree vlan [vlan-id] root primary
  ```
- **Configurer un Root secondaire** (priorité légèrement plus haute) :
  ```bash
  Switch(config)# spanning-tree vlan [vlan-id] root secondary
  ```

