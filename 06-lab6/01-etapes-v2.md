### Module 6 - Lab Guidé : Création d'un Cloud Privé Virtuel (VPC)

#### Vue d'ensemble et objectifs du lab

Ce lab vous guide à travers la création et la gestion d'un Virtual Private Cloud (VPC) sur AWS. Vous allez configurer un VPC, créer des sous-réseaux, déployer une passerelle Internet, configurer des tables de routage, créer un groupe de sécurité, et lancer un serveur d'application. À la fin du lab, vous devriez être capable de :

- Déployer un VPC
- Créer une passerelle Internet et l'attacher au VPC
- Créer un sous-réseau public
- Créer un sous-réseau privé
- Lancer un serveur d'application pour tester le VPC

**Durée :** Environ 30 minutes

---

### Liste des Étapes et Résultats

1. **Création du VPC :**
   - **[Task 1]** Créez un VPC nommé `Lab VPC` avec une plage CIDR de `10.0.0.0/16`. **(Score : 5/5)**

2. **Création des sous-réseaux :**
   - **[Task 2A]** Créez un sous-réseau public avec une plage CIDR de `10.0.0.0/24`. **(Score : 5/5)**
   - **[Task 2B]** Activez l'auto-attribution des adresses IP publiques pour le sous-réseau public. **(Score : 0/5)**
   - **[Task 2C]** Créez un sous-réseau privé avec une plage CIDR de `10.0.2.0/23`. **(Score : 5/5)**

3. **Création d'une passerelle Internet :**
   - **[Task 3]** Créez une passerelle Internet nommée `Lab IGW` et attachez-la au VPC. **(Score : 5/5)**

4. **Configuration des tables de routage :**
   - **[Task 4A]** Assurez-vous que le VPC dispose d'une table de routage privée. **(Score : 0/5)**
   - **[Task 4B]** Assurez-vous que le sous-réseau public dispose d'une table de routage publique. **(Score : 0/5)**

5. **Création d'un groupe de sécurité :**
   - **[Task 5]** Créez un groupe de sécurité `App-SG` pour permettre le trafic HTTP (port 80) depuis n'importe où. **(Score : 5/5)**

6. **Lancement d'un serveur d'application :**
   - Lancez une instance EC2 dans le sous-réseau public avec le groupe de sécurité `App-SG` et configurez-la pour exécuter une application web.

7. **Vérification de la configuration :**
   - Vérifiez que l'application est accessible via l'adresse IP publique de l'instance EC2.

8. **Soumission du travail :**
   - Soumettez votre travail via l'interface du lab et terminez le lab en cliquant sur `End Lab`.

---

### Schéma ASCI du VPC

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

**Score total : 25/45**

Vous avez bien réalisé certaines étapes comme la création du VPC, des sous-réseaux, et du groupe de sécurité, mais d'autres étapes, notamment la configuration des tables de routage et l'auto-attribution des IP publiques, n'ont pas été correctement effectuées.
