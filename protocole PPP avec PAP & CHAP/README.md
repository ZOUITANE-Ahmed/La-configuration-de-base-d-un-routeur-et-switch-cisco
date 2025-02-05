# 🔹 **Configuration du protocole PPP avec PAP & CHAP sur un routeur Cisco**  

## 📌 **1. Introduction à PPP**  
**PPP (Point-to-Point Protocol)** est un protocole de couche 2 utilisé pour établir une connexion point à point entre deux routeurs via une liaison série ou un lien encapsulé. Il prend en charge des fonctionnalités avancées comme :
- **Authentification (PAP et CHAP)**
- **Compression des données**
- **Encapsulation multi-protocoles**
- **Détection des erreurs**

Dans cette configuration, nous verrons comment **configurer PPP avec l'authentification PAP et CHAP** sur un routeur Cisco.

---

## 📌 **2. Différences entre PAP et CHAP**  

| **Méthode** | **PAP (Password Authentication Protocol)** | **CHAP (Challenge Handshake Authentication Protocol)** |
|------------|----------------------------------|-----------------------------------|
| **Sécurité** | Moins sécurisé (mot de passe envoyé en clair) | Plus sécurisé (challenge-réponse, mot de passe haché) |
| **Authentification** | Unidirectionnelle (le client s'authentifie auprès du serveur) | Bidirectionnelle (challenge-réponse mutuel) |
| **Fréquence** | Une seule fois à la connexion | Périodiquement pendant la session |
| **Vulnérabilités** | Vulnérable aux attaques d'écoute | Protégé contre le rejeu et l’interception |

---

## 📌 **3. Topologie pour la Configuration**  
Nous avons **deux routeurs** connectés via une liaison série (**Serial 0/0/0** sur chaque routeur) :

```
[R1] -------- [R2]
```

---

## 📌 **4. Configuration de PPP avec PAP**
### **🔹 Étape 1 : Définir les Identifiants sur Chaque Routeur**
Sur **R1** :
```bash
R1(config)# username R2 password cisco123  # Définir un utilisateur pour R2
```
Sur **R2** :
```bash
R2(config)# username R1 password cisco123  # Définir un utilisateur pour R1
```

---

### **🔹 Étape 2 : Configurer l'Interface Série avec PPP et PAP**
Sur **R1** :
```bash
R1(config)# interface Serial0/0/0
R1(config-if)# encapsulation ppp  # Activer PPP sur l'interface
R1(config-if)# ppp authentication pap  # Activer l'authentification PAP
R1(config-if)# ppp pap sent-username R1 password cisco123  # Envoyer l'identifiant et le mot de passe
R1(config-if)# no shutdown
```
Sur **R2** :
```bash
R2(config)# interface Serial0/0/0
R2(config-if)# encapsulation ppp
R2(config-if)# ppp authentication pap
R2(config-if)# ppp pap sent-username R2 password cisco123
R2(config-if)# no shutdown
```

---

## 📌 **5. Configuration de PPP avec CHAP**
CHAP est **plus sécurisé** car il utilise une méthode de hachage pour l’authentification.

### **🔹 Étape 1 : Définir les Identifiants sur Chaque Routeur**
Sur **R1** :
```bash
R1(config)# username R2 password cisco123
```
Sur **R2** :
```bash
R2(config)# username R1 password cisco123
```

---

### **🔹 Étape 2 : Configurer l'Interface Série avec PPP et CHAP**
Sur **R1** :
```bash
R1(config)# interface Serial0/0/0
R1(config-if)# encapsulation ppp  # Activer PPP
R1(config-if)# ppp authentication chap  # Activer CHAP
R1(config-if)# no shutdown
```
Sur **R2** :
```bash
R2(config)# interface Serial0/0/0
R2(config-if)# encapsulation ppp
R2(config-if)# ppp authentication chap
R2(config-if)# no shutdown
```

---

## 📌 **6. Vérification et Dépannage**
### **🔹 Vérifier que PPP est actif**
```bash
R1# show interfaces Serial0/0/0
```
Si PPP fonctionne correctement, la sortie devrait inclure :
```
Encapsulation PPP, LCP Open
```

---

### **🔹 Vérifier l'authentification PPP**
```bash
R1# debug ppp authentication
```
Si l'authentification échoue, assure-toi que :
- Les **noms d'utilisateur et mots de passe** sont identiques sur les deux routeurs.
- L’option **ppp authentication pap** ou **ppp authentication chap** est bien activée.
- L’interface série est bien **up (no shutdown)**.

---

## ✅ **Résumé des Commandes Importantes**
| **Commande** | **Description** |
|--------------|-----------------|
| `encapsulation ppp` | Active PPP sur une interface |
| `ppp authentication pap` | Active l’authentification PAP |
| `ppp authentication chap` | Active l’authentification CHAP |
| `ppp pap sent-username <nom> password <mdp>` | Envoie le nom d’utilisateur et le mot de passe pour PAP |
| `username <nom> password <mdp>` | Définit un utilisateur pour l’authentification PAP/CHAP |
| `show interfaces Serial0/0/0` | Vérifie l’état de l’interface série |
| `debug ppp authentication` | Débogue l’authentification PPP |

---

## 🎯 **Conclusion**
- **PAP** est simple mais **peu sécurisé** (envoie le mot de passe en clair).
- **CHAP** est **plus sécurisé** (utilise un challenge-réponse).
- **PPP** est un protocole de **couche 2 puissant** qui permet l'authentification et la compression des données.
