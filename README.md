# La-configuration-de-base-d-un-routeur-et-switch-cisco

Voici un résumé structuré des commandes de base pour la configuration des routeurs et commutateurs Cisco :

### Commandes de Base pour la Configuration Cisco

#### 1. **Passer au Mode Privilégié**
```bash
Router> enable
Switch> enable
```

#### 2. **Passer au Mode de Configuration Globale**
```bash
Router# configure terminal
Switch# configure terminal
```

#### 3. **Renommer le Nom d'Hôte**
```bash
Router(config)# hostname NouveauNom
Switch(config)# hostname NouveauNom
```

#### 4. **Configurer l'Interface FastEthernet**
```bash
Router(config)# interface FastEthernet [numéro]
Router(config-if)# ip address [@] [masque de sous-réseau]
Router(config-if)# no shutdown
Router(config-if)# exit
```

#### 5. **Configurer l'Interface Serial**
```bash
Router(config)# interface serial [numéro]
Router(config-if)# ip address [@] [masque]
Router(config-if)# clock rate [nombre]
Router(config-if)# no shutdown
Router(config-if)# exit
```

#### 6. **Attribuer un Mot de Passe à l'Accès par Terminal**
```bash
Router(config)# line console 0
Router(config-line)# password [mot de passe]
Router(config-line)# login
Router(config-line)# exit
```

#### 7. **Attribuer un Mot de Passe à l'Accès par Telnet**
```bash
Router(config)# line vty 0 4
Router(config-line)# password [mot de passe]
Router(config-line)# login
Router(config-line)# exit
```

#### 8. **Configurer les Lignes VTY pour SSH**
```bash
Router(config)# line vty 0 4
Router(config-line)# transport input ssh
Router(config-line)# login local
Router(config-line)# exit
```

#### 9. **Configurer le Domaine**
```bash
Router(config)# ip domain-name exemple.ma
```

#### 10. **Générer une Clé RSA pour le Chiffrement SSH**
```bash
Router(config)# crypto key generate rsa
```

#### 11. **Forcer l'Utilisation de SSH Version 2**
```bash
Router(config)# ip ssh version 2
```

Ces commandes couvrent les principales configurations nécessaires pour mettre en place un routeur ou un commutateur Cisco.
