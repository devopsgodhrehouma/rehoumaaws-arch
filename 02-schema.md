
```
+-------------------------+                        +-------------------------+
|       Utilisateur        |                        |  Administrateur (SSH)   |
|       (Navigateur)       |                        |                         |
+-----------+--------------+                        +------------+------------+
            |                                                   |
            |                                                   |
            v                                                   v
+-----------+---------------------------------------------------+-------------+
|                               Internet                                     |
+---------------+-------------------------------------------+----------------+
                |                                           |
                |                                           |
                v                                           v
     +----------+-------------------+           +-----------+---------------+
     |  Équilibreur de Charge (ELB) |           |          Sous-Réseau       |
     |                              |           |           Public           |
     +----------+-------------------+           +-----------+---------------+
                |                                           |
                |                                           |
                v                                           v
     +----------+-------------------+           +-----------+---------------+
     | Groupe de Mise à l'Échelle   |           | t2.micro EC2               |
     |    (Plusieurs Instances EC2) |           | (Application PHP)          |
     +----------+-------------------+           +-----------+---------------+
                |                                           |
                |                                           |
                v                                           |
+---------------+--------------------+-----------+----------+---------------+
|                      Sous-Réseau Privé                                            |
|    +--------------------------+             +---------------------------+       |
|    |  Base de Données RDS      |             |  Stockage de Paramètres SSM  |      |
|    |        MySQL              |             +---------------------------+       |
|    +--------------------------+             |   |                                  |
|           |                                  |   +--------------------+------------|
|           +----------------------------------+------------------------+            |
|                                                |                                   |
|                                                v                                   |
|                                    +-----------+------------+                    |
|                                    |  Clés de Paramètres    |                    |
|                                    |  (Endpoint, Utilisateur,|                    |
|                                    |   Mot de Passe, Base)   |                    |
|                                    +------------------------+                    |
+-------------------------------------------------------------------------------+
```

### Explication :
- **Utilisateur (Navigateur)** : Représente les utilisateurs accédant au site web.
- **Administrateur (SSH)** : Représente l'accès administratif via SSH à l'instance EC2.
- **Équilibreur de Charge (ELB)** : Distribue le trafic entrant sur plusieurs instances EC2.
- **Groupe de Mise à l'Échelle (Auto Scaling Group)** : Gère un groupe d'instances EC2, les faisant évoluer en fonction de la demande.
- **Sous-Réseau Public** : Contient l'instance EC2 exécutant l'application PHP, accessible publiquement.
- **Sous-Réseau Privé** : Contient la base de données RDS MySQL, qui n'est pas accessible publiquement, garantissant ainsi la sécurité. Le Stockage de Paramètres SSM se trouve également ici, stockant les informations sensibles comme les détails de connexion à la base de données.
- **Stockage de Paramètres SSM** : Stocke les données de configuration de manière sécurisée (comme l'endpoint de la base de données, le nom d'utilisateur et le mot de passe) que l'application PHP peut récupérer.

