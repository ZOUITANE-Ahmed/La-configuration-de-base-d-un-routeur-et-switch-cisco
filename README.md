**commandes de base** pour la **configuration d’un switch et d’un routeur Cisco** en ligne de commande (CLI), utiles dans un contexte d’apprentissage, de laboratoire ou d’administration réseau :

---

### 🔌 1. Configuration d’un **Switch Cisco**

#### ➤ Accès au switch
```bash
Switch> enable
Switch# configure terminal
```

#### ➤ Renommer le switch
```bash
Switch(config)# hostname Switch1
```

#### ➤ Désactiver la recherche DNS (évite les lenteurs si commande erronée)
```bash
Switch1(config)# no ip domain-lookup
```

#### ➤ Configuration d’un mot de passe pour l’accès en mode privilégié
```bash
Switch1(config)# enable secret Cisco123
```

#### ➤ Configuration de la ligne console
```bash
Switch1(config)# line console 0
Switch1(config-line)# password cisco
Switch1(config-line)# login
Switch1(config-line)# exit
```

#### ➤ Configuration de la ligne VTY (accès SSH/Telnet)
```bash
Switch1(config)# line vty 0 4
Switch1(config-line)# password cisco
Switch1(config-line)# login
Switch1(config-line)# exit
```

#### ➤ Configuration d’une adresse IP sur une interface VLAN
```bash
Switch1(config)# interface vlan 1
Switch1(config-if)# ip address 192.168.1.2 255.255.255.0
Switch1(config-if)# no shutdown
Switch1(config-if)# exit
```

#### ➤ Définir la passerelle par défaut
```bash
Switch1(config)# ip default-gateway 192.168.1.1
```

#### ➤ Enregistrer la configuration
```bash
Switch1# write memory
# ou
Switch1# copy running-config startup-config
```

---

### 🌐 2. Configuration d’un **Routeur Cisco**

#### ➤ Accès au routeur
```bash
Router> enable
Router# configure terminal
```

#### ➤ Renommer le routeur
```bash
Router(config)# hostname R1
```

#### ➤ Configuration de l’interface (exemple : GigabitEthernet 0/0)
```bash
R1(config)# interface gigabitEthernet 0/0
R1(config-if)# ip address 192.168.1.1 255.255.255.0
R1(config-if)# no shutdown
R1(config-if)# exit
```

#### ➤ Configuration d’une autre interface (ex : réseau WAN)
```bash
R1(config)# interface gigabitEthernet 0/1
R1(config-if)# ip address 10.0.0.1 255.255.255.252
R1(config-if)# no shutdown
R1(config-if)# exit
```

#### ➤ Ajouter une route statique
```bash
R1(config)# ip route 192.168.2.0 255.255.255.0 10.0.0.2
```

#### ➤ Configuration de NAT/PAT (selon contexte)
```bash
# Exemple simplifié
R1(config)# ip nat inside source list 1 interface gigabitEthernet 0/1 overload
R1(config)# access-list 1 permit 192.168.1.0 0.0.0.255
R1(config)# interface gigabitEthernet 0/0
R1(config-if)# ip nat inside
R1(config-if)# exit
R1(config)# interface gigabitEthernet 0/1
R1(config-if)# ip nat outside
```

#### ➤ Enregistrer la configuration
```bash
R1# write memory
# ou
R1# copy running-config startup-config
```

---

### 🛠️ Astuce
Pour **vérifier** les configurations :
```bash
# Voir les interfaces
show ip interface brief

# Voir la configuration en cours
show running-config

# Voir la table de routage
show ip route
```
