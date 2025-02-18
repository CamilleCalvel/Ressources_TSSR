# Routage Inter-VLAN

Le routage inter-VLAN est une technique permettant aux VLANs (Virtual Local Area Networks) de communiquer entre eux. Chaque VLAN étant un réseau logique distinct, un dispositif capable de router les paquets entre eux est nécessaire.

## 1️⃣ Routage avec un routeur et des interfaces physiques distinctes

### 🔹 Principe
- Chaque VLAN est connecté à une **interface physique dédiée** sur le routeur.
- Chaque interface est configurée avec une adresse IP correspondant au sous-réseau du VLAN.
- Le routage entre VLANs est effectué par le routeur.

### ✅ Avantages
- Facile à configurer et comprendre.
- Convient aux petits réseaux.

### ❌ Inconvénients
- Nécessite autant d’interfaces physiques que de VLANs, ce qui est coûteux.
- Manque de scalabilité pour les grands réseaux.

---

## 2️⃣ Routage avec un routeur et une interface trunk (Router-on-a-Stick)

### 🔹 Principe
- Une seule **interface physique** du routeur est utilisée.
- Elle est configurée en mode **trunk** pour transporter le trafic de plusieurs VLANs.
- Le routeur utilise des **sous-interfaces** avec **802.1Q** pour identifier les VLANs.

### ✅ Avantages
- Économise les interfaces physiques du routeur.
- Solution évolutive pour les réseaux de taille moyenne.

### ❌ Inconvénients
- Peut devenir un **goulot d’étranglement** sur des réseaux à fort trafic.
- Dépendance sur la capacité du routeur à gérer le trafic inter-VLAN.

---

## 3️⃣ Routage avec un switch de niveau 3 (Layer 3 Switch)

### 🔹 Principe
- Utilisation d’un **switch de niveau 3** capable d’effectuer du routage.
- Chaque VLAN est configuré avec une **interface VLAN (SVI - Switched Virtual Interface)** qui sert de passerelle.
- Le switch route directement le trafic entre VLANs sans passer par un routeur externe.

### ✅ Avantages
- **Performances élevées** (le routage est fait au niveau matériel).
- **Scalabilité** (idéal pour les réseaux d’entreprise).
- **Réduction du trafic sur les liaisons trunk** et diminution de la latence.

### ❌ Inconvénients
- Nécessite un **switch de niveau 3**, qui est plus coûteux qu’un switch de niveau 2.

---

## 4️⃣ Routage avec SDN (Software-Defined Networking)

### 🔹 Principe
- Utilisation d’un **contrôleur SDN** pour gérer le routage inter-VLAN via des règles définies par logiciel.
- Exemples de solutions : OpenFlow, Cisco ACI.

### ✅ Avantages
- Gestion centralisée du routage et des règles de sécurité.
- Grande flexibilité et adaptation aux réseaux virtualisés et au cloud.

### ❌ Inconvénients
- Plus complexe à mettre en place.
- Nécessite des équipements compatibles SDN.

---

## 🏆 Quelle méthode choisir ?

| **Méthode** | **Utilisation recommandée** |
|------------|--------------------------|
| Routeur avec interfaces physiques | Petits réseaux avec peu de VLANs |
| Routeur-on-a-stick | Réseaux de taille moyenne avec un switch manageable |
| Switch de niveau 3 | Grandes infrastructures avec besoins de performances |
| SDN | Réseaux évolués, virtualisés ou cloud |

Si vous souhaitez un exemple de configuration sur Cisco ou Linux, n’hésitez pas à demander ! 😊

