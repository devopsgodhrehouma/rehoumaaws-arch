### Module 6 - Lab Guidé : Création d'un Cloud Privé Virtuel (VPC)

#### Vue d'ensemble et objectifs du lab

Ce lab vous apprendra à créer et gérer un Virtual Private Cloud (VPC) sur AWS. Vous allez configurer un VPC, créer des sous-réseaux, déployer une passerelle Internet, configurer des tables de routage, créer un groupe de sécurité et lancer un serveur d'application. À la fin du lab, vous devriez être capable de :

- Déployer un VPC
- Créer une passerelle Internet et l'attacher au VPC
- Créer un sous-réseau public
- Créer un sous-réseau privé
- Lancer un serveur d'application pour tester le VPC

**Durée:** Environ 30 minutes

---

### Restrictions des services AWS

Dans cet environnement de lab, l'accès aux services AWS est limité à ceux nécessaires pour accomplir les tâches du lab. Des erreurs peuvent survenir si vous tentez d'accéder à d'autres services ou d'effectuer des actions non autorisées.

---

### Étapes Principales du Lab

1. **Création d'un VPC :**
   - Créez un VPC nommé `Lab VPC` avec une plage CIDR de `10.0.0.0/16`.
   - Activez les noms d'hôtes DNS pour les instances EC2.

2. **Création des sous-réseaux :**
   - Créez un sous-réseau public (`10.0.0.0/24`) et un sous-réseau privé (`10.0.2.0/23`) dans le VPC.

3. **Création d'une passerelle Internet :**
   - Créez une passerelle Internet nommée `Lab IGW` et attachez-la au VPC.

4. **Configuration des tables de routage :**
   - Créez une table de routage publique, ajoutez une route vers la passerelle Internet, et associez-la au sous-réseau public.

5. **Création d'un groupe de sécurité :**
   - Créez un groupe de sécurité `App-SG` pour permettre le trafic HTTP (port 80) depuis n'importe où.

6. **Lancement d'un serveur d'application :**
   - Lancez une instance EC2 dans le sous-réseau public avec le groupe de sécurité `App-SG`, et configurez-la pour exécuter une application web.

7. **Vérification de la configuration :**
   - Vérifiez que l'application est accessible via l'adresse IP publique de l'instance EC2.

8. **Soumission du travail :**
   - Soumettez votre travail via l'interface du lab et terminez le lab en cliquant sur `End Lab`.

---

### Schéma ASCI du VPC

Voici un schéma simplifié de votre architecture après la réalisation du lab :

```plaintext
+-----------------------------------------------------------+
|                        Lab VPC (10.0.0.0/16)              |
|                                                           |
|   +------------------+     +---------------------------+ |
|   | Public Subnet     |     | Private Subnet            | |
|   | 10.0.0.0/24       |     | 10.0.2.0/23               | |
|   |                  |     |                           | |
|   |  +-------------+ |     |  +---------------------+  | |
|   |  | EC2 Instance | |     |  | Private Resources   |  | |
|   |  | (App Server) | |     |  | (DB, etc.)          |  | |
|   |  +-------------+ |     |  +---------------------+  | |
|   |                  |     |                           | |
|   +------------------+     +---------------------------+ |
|                                                           |
|  +-----------------------------+                          |
|  | Internet Gateway (Lab IGW)   |                         |
|  +-----------------------------+                         |
+-----------------------------------------------------------+
```
