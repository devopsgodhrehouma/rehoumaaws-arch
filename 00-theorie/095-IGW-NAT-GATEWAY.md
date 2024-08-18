# 1 - Définitions

#### 1. **Internet Gateway (IGW)**
Une **Internet Gateway (IGW)** est un composant essentiel pour permettre à un Virtual Private Cloud (VPC) sur AWS de communiquer avec Internet. Une IGW permet la communication bidirectionnelle : les instances situées dans des sous-réseaux publics peuvent envoyer du trafic vers Internet et recevoir des réponses depuis Internet. Pour qu'une instance puisse accéder à Internet, elle doit être dans un sous-réseau avec une table de routage contenant une route pointant vers l'IGW.

- **Rôle principal** : Faciliter la connectivité entre les instances dans un VPC et Internet.
- **Utilisation typique** : Connecter des serveurs web ou des applications frontales qui nécessitent une communication directe avec Internet.

#### 2. **NAT Gateway Publique**
Une **NAT Gateway Publique** est un service géré par AWS qui permet aux instances situées dans des sous-réseaux privés d'accéder à Internet pour envoyer du trafic sortant, comme les mises à jour logicielles, sans exposer ces instances à des connexions entrantes depuis Internet. La NAT Gateway est déployée dans un sous-réseau public et doit avoir une adresse IP publique associée pour fonctionner.

- **Rôle principal** : Permettre le trafic sortant depuis des sous-réseaux privés vers Internet tout en empêchant les connexions entrantes non sollicitées.
- **Utilisation typique** : Instances dans des sous-réseaux privés qui doivent accéder à Internet pour télécharger des mises à jour ou accéder à des services externes.

#### 3. **NAT Gateway Privée**
Une **NAT Gateway Privée** est un concept souvent utilisé pour décrire une configuration où une NAT Gateway est utilisée pour la communication entre des sous-réseaux privés ou entre un VPC et d'autres réseaux privés, sans connexion directe à Internet. En pratique, AWS ne propose pas une "NAT Gateway Privée" en tant que service distinct. Toutefois, dans certaines configurations spécifiques, une NAT Gateway peut être utilisée pour le routage interne sans permettre l'accès à Internet.

- **Rôle principal** : Faciliter la communication entre les sous-réseaux privés au sein d'un VPC ou entre un VPC et un autre réseau privé sans connexion directe à Internet.
- **Utilisation typique** : Communication entre instances dans des sous-réseaux privés ou entre différents VPCs.

#### 4. **Tables de Routage**
Les **Tables de Routage** sont des composants critiques du réseau qui déterminent comment le trafic est acheminé au sein d'un VPC. Chaque sous-réseau dans un VPC est associé à une table de routage, qui contient des règles de routage. Ces règles spécifient les destinations possibles (comme une adresse IP ou une plage d'adresses) et les cibles (comme une passerelle Internet, une NAT Gateway, ou un autre sous-réseau) vers lesquelles le trafic doit être dirigé.

- **Rôle principal** : Déterminer le chemin que doit suivre le trafic réseau pour atteindre sa destination.
- **Utilisation typique** : Diriger le trafic Internet via une IGW, acheminer le trafic privé via une NAT Gateway, ou permettre la communication interne entre sous-réseaux.

### Schémas de Routage et Configurations

Maintenant que nous avons défini les composants, passons aux schémas ASCII pour illustrer comment ces éléments fonctionnent ensemble et comment configurer les tables de routage.

#### Exemple de Routage avec une Internet Gateway (IGW)

```
+----------------------+                 +----------------------+
|   Public Subnet 1    |                 |   Public Subnet 2    |
| 10.0.0.0/24          |                 | 10.1.0.0/24          |
|                      |                 |                      |
|  [Public Instance]   |                 | [Public Instance]    |
|                      |                 |                      |
+----------+-----------+                 +----------+-----------+
           |                                     |
           |                                     |
           +--------------------+----------------+
                                |
                         +------+-------+
                         |  Internet    |
                         |  Gateway     |
                         +------+-------+
                                |
                                |
                     Route Table for Public Subnet
                                |
                     Destination: 0.0.0.0/0 --> IGW
                                |
                                |
                              Internet
```

**Table de Routage Exemple** :
```
Destination        | Cible
-------------------|-----------------
10.0.0.0/16        | Local
0.0.0.0/0          | IGW (Internet Gateway)
```

#### Exemple de Routage avec une NAT Gateway Publique

```
+----------------------+                 +----------------------+
|   Public Subnet 1    |                 |   Private Subnet 1   |
| 10.0.0.0/24          |                 | 10.0.2.0/24          |
|                      |                 |                      |
|  [NAT Gateway]       |                 | [Private Instance]   |
|                      |                 |                      |
+----------+-----------+                 +----------+-----------+
           |                                     |
           |                                     |
           +--------------------+                |
                                |                |
                         +------+-------+        |
                         |  Internet    |        |
                         |  Gateway     |        |
                         +------+-------+        |
                                |                |
                                |                |
                     Route Table for Public      |   Route Table for Private
                     Subnet:                     |   Subnet:
                     Destination: 0.0.0.0/0      |   Destination: 0.0.0.0/0 
                     --> IGW                     |   --> NAT Gateway
                                |                |
                                |                |
                              Internet           |
```

**Table de Routage Exemple** :
```
Destination        | Cible
-------------------|-----------------
10.0.0.0/16        | Local
0.0.0.0/0          | NAT Gateway (dans Public Subnet)
```
- Ces exemples montrent comment configurer vos tables de routage pour diriger le trafic correctement en utilisant les composants définis. 
- Les tables de routage sont cruciales pour s'assurer que le trafic est envoyé vers les bonnes destinations, qu'il s'agisse d'Internet, d'autres sous-réseaux, ou d'autres réseaux privés.

---

# 2 - Shéma et scénario 

Pour visualiser la configuration des **Internet Gateway (IGW)**, **NAT Gateway privée**, **NAT Gateway publique**, et les **tables de routage** associées, voici des exemples de configurations.

### 1. **Schéma avec Internet Gateway (IGW)**

```
+----------------------+                 +----------------------+
|   Public Subnet 1    |                 |   Public Subnet 2    |
| 10.0.0.0/24          |                 | 10.1.0.0/24          |
|                      |                 |                      |
|  [Public Instance]   |                 | [Public Instance]    |
|                      |                 |                      |
+----------+-----------+                 +----------+-----------+
           |                                     |
           |                                     |
           +--------------------+----------------+
                                |
                         +------+-------+
                         |  Internet    |
                         |  Gateway     |
                         +------+-------+
                                |
                                |
                     Route Table for Public Subnet
                                |
                     Destination: 0.0.0.0/0 --> IGW
                                |
                                |
                              Internet
```

### 2. **Schéma avec NAT Gateway Publique**

```
+----------------------+                 +----------------------+
|   Public Subnet 1    |                 |   Private Subnet 1   |
| 10.0.0.0/24          |                 | 10.0.2.0/24          |
|                      |                 |                      |
|  [NAT Gateway]       |                 | [Private Instance]   |
|                      |                 |                      |
+----------+-----------+                 +----------+-----------+
           |                                     |
           |                                     |
           +--------------------+                |
                                |                |
                         +------+-------+        |
                         |  Internet    |        |
                         |  Gateway     |        |
                         +------+-------+        |
                                |                |
                                |                |
                     Route Table for Public      |   Route Table for Private
                     Subnet:                     |   Subnet:
                     Destination: 0.0.0.0/0      |   Destination: 0.0.0.0/0 
                     --> IGW                     |   --> NAT Gateway
                                |                |
                                |                |
                              Internet           |
```

### 3. **Schéma avec NAT Gateway Privée**

```
+----------------------+                 +----------------------+
|   Private Subnet 1   |                 |   Private Subnet 2   |
| 10.0.2.0/24          |                 | 10.0.3.0/24          |
|                      |                 |                      |
|  [Private Instance]  |                 | [Private Instance]   |
|                      |                 |                      |
+----------+-----------+                 +----------+-----------+
           |                                     |
           |                                     |
           +--------------------+                |
                                |                |
                         +------+-------+        |
                         |  NAT Gateway |        |
                         |  (Privée)    |        |
                         +------+-------+        |
                                |                |
                                |                |
                     Route Table for Private     |   Route Table for Private
                     Subnet:                     |   Subnet:
                     Destination:                |   Destination:
                     10.0.3.0/24 -->             |   10.0.2.0/24 --> NAT Gateway
                     --> NAT Gateway             |                |
                                |                |
                                |                |
                              VPC Internal       |
```

### Exemples de Tables de Routage
Voici comment vous pouvez configurer les tables de routage pour chaque sous-réseau:

1. **Table de Routage pour le Public Subnet (avec IGW)** :
   ```
   Destination        | Cible
   -------------------|-----------------
   10.0.0.0/16        | Local
   0.0.0.0/0          | IGW (Internet Gateway)
   ```

2. **Table de Routage pour le Private Subnet (avec NAT Gateway publique)** :
   ```
   Destination        | Cible
   -------------------|-----------------
   10.0.0.0/16        | Local
   0.0.0.0/0          | NAT Gateway (dans Public Subnet)
   ```

3. **Table de Routage pour le Private Subnet (avec NAT Gateway privée)** :
   ```
   Destination        | Cible
   -------------------|-----------------
   10.0.2.0/24        | Local
   10.0.3.0/24        | NAT Gateway
   ```

### Explication des Configurations

- **IGW (Internet Gateway)** : Permet aux instances dans un sous-réseau public de se connecter à Internet. La table de routage du sous-réseau public contient une route `0.0.0.0/0` pointant vers l'IGW.
  
- **NAT Gateway Publique** : Placée dans un sous-réseau public, elle permet aux instances dans un sous-réseau privé d'accéder à Internet pour les mises à jour et autres communications sortantes, sans exposer ces instances à Internet. La table de routage du sous-réseau privé contiendra une route `0.0.0.0/0` pointant vers la NAT Gateway.

- **NAT Gateway Privée** : Permet la communication entre sous-réseaux privés sans exposer les instances à Internet. Cela est souvent utilisé pour la communication interne au sein du VPC ou entre VPC via des connexions comme le VPC Peering ou le Direct Connect.

Ces configurations assurent que les ressources sont accessibles de manière sécurisée en fonction de leur besoin de communication avec Internet ou d'autres réseaux.

