Voici les propriétés, les avantages et les commandes des protocoles FHRP (First Hop Redundancy Protocols) : **VRRP** (Virtual Router Redundancy Protocol), **HSRP** (Hot Standby Router Protocol) et **GLBP** (Gateway Load Balancing Protocol).

### 1. **VRRP (Virtual Router Redundancy Protocol)**
#### Propriétés
- **Normalisé** : VRRP est un protocole standardisé par l'IETF (RFC 5798).
- **Adressage IP Virtuel** : Il permet à plusieurs routeurs de partager une adresse IP virtuelle, où l'un des routeurs est élu comme maître.

#### Avantages
- **Interopérabilité** : Puisqu'il est standardisé, il fonctionne avec différents équipements de différents fabricants.
- **Basé sur les priorités** : La redondance est déterminée par la priorité, et le routeur ayant la plus haute priorité devient le maître.

#### Commandes de base (Cisco IOS)
```bash
interface GigabitEthernet0/1
  vrrp 1 ip 192.168.1.1
  vrrp 1 priority 120
  vrrp 1 preempt
```

### 2. **HSRP (Hot Standby Router Protocol)**
#### Propriétés
- **Propriétaire** : HSRP est un protocole propriétaire de Cisco.
- **État de veille actif/secondaire** : Un routeur est actif, tandis que l'autre reste en mode veille, prêt à prendre la relève si nécessaire.

#### Avantages
- **Facile à configurer** : Idéal pour les réseaux simples où une redondance basique est nécessaire.
- **Suivi d'interface** : Possibilité de suivre les interfaces pour détecter les pannes de routeur et basculer automatiquement vers le routeur de secours.

#### Commandes de base (Cisco IOS)
```bash
interface GigabitEthernet0/1
  standby 1 ip 192.168.1.1
  standby 1 priority 110
  standby 1 preempt
```

### 3. **GLBP (Gateway Load Balancing Protocol)**
#### Propriétés
- **Propriétaire** : GLBP est également un protocole propriétaire de Cisco.
- **Équilibrage de charge** : GLBP permet l'équilibrage de charge entre plusieurs routeurs, en répartissant le trafic sur plusieurs passerelles.

#### Avantages
- **Distribution du trafic** : Capacité à utiliser plusieurs passerelles de manière simultanée, améliorant ainsi la répartition du trafic.
- **Gestion de l'état de veille** : Comme HSRP, GLBP prend en charge la bascule en cas de panne, tout en permettant l'équilibrage de charge.

#### Commandes de base (Cisco IOS)
```bash
interface GigabitEthernet0/1
  glbp 1 ip 192.168.1.1
  glbp 1 priority 100
  glbp 1 preempt
```

Ces protocoles permettent tous d'assurer la redondance et la haute disponibilité dans les réseaux, mais ils diffèrent par leur fonctionnement, leurs avantages et leurs options de configuration.
