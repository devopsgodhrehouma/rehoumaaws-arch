

# 1 - Architecture - lab9-défi

![image](https://github.com/user-attachments/assets/47219fa1-dd0c-461b-9050-c6fd49bb2108)


# 2 - Tableau des composants de l'architecture

| **Composant**              | **Description**                                                                                                                                                     |
|----------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **VPC (Virtual Private Cloud)**          | Un VPC avec une plage CIDR de `10.0.0.0/16`, couvrant deux zones de disponibilité (AZ) pour garantir la redondance et la tolérance aux pannes.                |
| **Zones de Disponibilité**  | Deux zones de disponibilité : AZ A et AZ B, pour garantir la haute disponibilité.                                                                                   |
| **Public Subnet 1**         | Sous-réseau public dans l'AZ A (`10.0.0.0/24`). Héberge la passerelle NAT pour permettre aux instances privées d'accéder à Internet.                                   |
| **Public Subnet 2**         | Sous-réseau public dans l'AZ B (`10.1.0.0/24`). Héberge également une passerelle NAT pour l'accès à Internet depuis les sous-réseaux privés de l'AZ B.               |
| **Private Subnet 1**        | Sous-réseau privé dans l'AZ A (`10.0.2.0/24`). Héberge les serveurs d'application du café.                                                                            |
| **Private Subnet 2**        | Sous-réseau privé dans l'AZ B (`10.1.2.0/24`). Héberge les serveurs d'application du café.                                                                            |
| **Private Subnet 3**        | Sous-réseau privé dans l'AZ A (`10.0.4.0/24`). Héberge l'instance principale de la base de données MySQL.                                                             |
| **Private Subnet 4**        | Sous-réseau privé dans l'AZ B (`10.1.4.0/24`). Peut être utilisé pour la redondance ou d'autres composants.                                                          |
| **Passerelle Internet**     | Une passerelle Internet connectée aux sous-réseaux publics, permettant aux ressources publiques de communiquer avec l'extérieur.                                     |
| **NAT Gateway**             | Passerelle NAT dans les sous-réseaux publics, permettant aux instances dans les sous-réseaux privés d'accéder à Internet tout en restant inaccessibles depuis l'extérieur. |
| **Load Balancer**           | Un équilibreur de charge (ELB) pour distribuer le trafic entre les instances d'application dans les sous-réseaux privés des deux AZ.                                  |
| **Auto Scaling Group**      | Un groupe d'auto-scaling pour gérer automatiquement le nombre d'instances d'application en fonction de la charge du serveur.                                         |
| **Base de Données MySQL**   | Instance de base de données MySQL hébergée dans le sous-réseau privé 3 pour le stockage des données des applications du café.                                         |
| **AMI (Amazon Machine Image)** | Image machine utilisée pour lancer des instances EC2 dans le groupe d'auto-scaling.                                                                                 |

### Explication en Français

Cette architecture est conçue pour assurer une haute disponibilité et une scalabilité automatique pour l'application web du café, en anticipant un pic de trafic en raison d'une apparition à la télévision. Voici une explication détaillée de chaque composant :

- **VPC** : La VPC est une infrastructure réseau isolée dans le cloud AWS où tous les autres composants sont déployés. Elle est configurée pour utiliser une large plage d'adresses IP (`10.0.0.0/16`), et s'étend sur deux zones de disponibilité (AZ) pour garantir que l'application reste disponible même en cas de défaillance dans l'une des zones.

- **Zones de Disponibilité** : L'utilisation de deux AZ (A et B) garantit que si une zone tombe en panne, l'autre peut prendre le relais, assurant ainsi la continuité du service.

- **Sous-réseaux Publics** : Ces sous-réseaux sont accessibles depuis Internet et hébergent des composants comme les passerelles NAT et le load balancer, qui nécessitent une connectivité externe. Les passerelles NAT permettent aux instances dans les sous-réseaux privés de communiquer avec Internet, par exemple pour télécharger des mises à jour, tout en restant inaccessibles depuis l'extérieur.

- **Sous-réseaux Privés** : Les serveurs d'application et la base de données sont hébergés dans des sous-réseaux privés, ce qui les isole d'Internet et les protège des menaces extérieures. Le trafic est acheminé vers ces serveurs via l'équilibreur de charge (ELB), qui répartit les requêtes des clients entre les serveurs pour éviter la surcharge et améliorer la réactivité.

- **Passerelle Internet** : Elle permet aux sous-réseaux publics de communiquer avec l'Internet, essentiel pour que le load balancer reçoive des requêtes de clients.

- **Load Balancer** : Il distribue le trafic entre les instances des serveurs d'application dans les sous-réseaux privés. Cela permet de gérer les pics de trafic en assurant une répartition équilibrée des requêtes.

- **Auto Scaling Group** : Ce groupe ajuste automatiquement le nombre d'instances EC2 en fonction de la demande. Par exemple, en cas de forte affluence sur le site web, de nouvelles instances seront automatiquement lancées pour absorber la charge.

- **Base de Données MySQL** : Stockée dans un sous-réseau privé pour plus de sécurité, la base de données conserve toutes les informations nécessaires au fonctionnement de l'application web, comme les commandes des clients.

- **AMI** : Cette image machine est utilisée pour déployer de nouvelles instances de serveurs d'application, assurant une cohérence dans la configuration des serveurs à chaque fois qu'un nouveau serveur est lancé.

Cette architecture est donc pensée pour offrir une expérience utilisateur fluide, même en cas de forte augmentation du trafic, grâce à une répartition efficace des ressources et une scalabilité automatique.

# 3 - Questions et Réponses

**Q1 : Private Subnet 4 peut-il communiquer avec Private Subnet 3 ? Si oui, via quel mécanisme ?**

**R1 :** Oui, Private Subnet 4 peut communiquer avec Private Subnet 3. La communication entre les sous-réseaux privés au sein du même VPC est possible via le routage interne du VPC, qui utilise une table de routage définissant les chemins de communication entre les sous-réseaux. 

**Q2 : Private Subnet 4 peut-il communiquer avec Public Subnet 2 ? Pourquoi et via quel mécanisme ?**

**R2 :** Oui, Private Subnet 4 peut communiquer avec Public Subnet 2. Cette communication est possible via le routage interne au sein du VPC, à condition que les tables de routage soient configurées pour permettre cette communication. Le routage interne dans le VPC permet aux instances dans différents sous-réseaux de se parler directement sans passer par l'extérieur.

**Q3 : Private Subnet 4 peut-il communiquer avec l'extérieur (Internet) ? Pourquoi ?**

**R3 :** Non, Private Subnet 4 ne peut pas communiquer directement avec l'extérieur (Internet) car il ne possède pas de passerelle Internet (Internet Gateway) associée. Pour accéder à Internet, les instances dans Private Subnet 4 doivent passer par une passerelle NAT située dans un sous-réseau public.

**Q4 : Pourquoi avons-nous deux Public Subnets ?**

**R4 :** Deux Public Subnets sont utilisés pour assurer la haute disponibilité. Chaque sous-réseau public est situé dans une zone de disponibilité différente (AZ A et AZ B). Cela garantit que si l'une des zones de disponibilité tombe en panne, les services hébergés dans l'autre zone resteront accessibles.

**Q5 : Pourquoi avons-nous deux Private Subnets ?**

**R5 :** Deux Private Subnets sont utilisés pour assurer la haute disponibilité des serveurs d'application. Chaque sous-réseau privé est situé dans une zone de disponibilité différente, permettant ainsi de répartir la charge et d'assurer la continuité du service en cas de défaillance d'une zone de disponibilité.

**Q6 : Quel est le rôle de la Passerelle Internet (Internet Gateway) ?**

**R6 :** La Passerelle Internet (Internet Gateway) permet aux instances situées dans les sous-réseaux publics de communiquer avec Internet. Elle assure également que les ressources externes peuvent accéder à ces instances, ce qui est essentiel pour les composants publics comme les équilibreurs de charge (load balancers).

**Q7 : Quel est le rôle de la NAT Gateway ?**

**R7 :** La NAT Gateway permet aux instances situées dans les sous-réseaux privés d'accéder à Internet pour des mises à jour ou autres communications sortantes, tout en empêchant les communications entrantes non sollicitées depuis l'Internet vers ces instances. Cela maintient la sécurité des ressources privées.

**Q8 : Le Load Balancer va-t-il créer chaque fois 3 nouvelles instances ?**

**R8 :** Non, le Load Balancer ne crée pas lui-même les instances. Les instances sont créées par le groupe d'Auto Scaling en fonction de la charge sur les serveurs. Le nombre d'instances créées dépend des règles de scaling définies (par exemple, lorsque la charge CPU dépasse un certain seuil). Le Load Balancer distribue simplement le trafic entre les instances existantes.

**Q9 : Dans quel sous-réseau les nouvelles instances seront-elles créées ?**

**R9 :** Les nouvelles instances seront créées dans les sous-réseaux privés (Private Subnet 1 et Private Subnet 2) spécifiés lors de la configuration du groupe d'Auto Scaling. Oui, vous pouvez choisir dans quels sous-réseaux les instances seront lancées en configurant les sous-réseaux disponibles dans le groupe d'Auto Scaling.

