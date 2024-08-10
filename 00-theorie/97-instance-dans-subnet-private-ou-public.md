# Questions: Nous devons mettre l'instance dans un sous réseaux publique ou privé ?

- Créer une instance dans un sous-réseau public n'est pas courant pour certaines raisons de sécurité, mais cela dépend du cas d'utilisation. Voici quelques points à considérer :

1. **Exposition à Internet :** Un sous-réseau public a une route vers une passerelle Internet, ce qui signifie que les instances peuvent être directement accessibles depuis l'Internet public si elles ont une adresse IP publique. Cela peut être risqué si l'instance n'est pas correctement sécurisée.

2. **Sécurité :** Pour minimiser les risques, les instances critiques ou sensibles sont généralement placées dans un sous-réseau privé, où elles ne sont pas directement accessibles depuis l'Internet. Les accès externes sont généralement gérés via un Bastion Host (dans le sous-réseau public) ou des VPN.

3. **Cas d'Utilisation Courant :** Les instances dans les sous-réseaux publics sont souvent des serveurs web ou des applications qui doivent être accessibles depuis l'extérieur. Pour le backend ou les bases de données, elles sont généralement placées dans des sous-réseaux privés.

En résumé, il est possible de créer une instance dans un sous-réseau public, mais ce n'est pas toujours recommandé sauf si l'accès direct à Internet est nécessaire et que toutes les mesures de sécurité sont en place.

# Annexe : **Sous-réseau Public - Considérations et Bonnes Pratiques**

## **Introduction**

Ce document aborde les considérations de sécurité et les bonnes pratiques à adopter lors de la création d'instances dans des sous-réseaux publics au sein d'une infrastructure cloud. Il est conçu pour guider les architectes cloud, les administrateurs réseau et les développeurs dans la gestion des risques associés à l'exposition de ressources sur Internet.

## **Pourquoi ce guide est-il important ?**

Créer une instance dans un sous-réseau public n'est pas courant pour plusieurs raisons, principalement liées à la sécurité. Ce guide explique les risques potentiels et fournit des recommandations pour garantir la sécurité et la conformité de votre infrastructure.

## **Points Clés à Considérer**

### **1. Exposition à Internet**

Un sous-réseau public a une route vers une passerelle Internet, ce qui signifie que les instances peuvent être directement accessibles depuis l'Internet public si elles disposent d'une adresse IP publique. Cela comporte des risques, notamment :

- **Attaques Directes :** Les instances exposées peuvent être la cible d'attaques comme le déni de service (DoS), le brute force, et autres.
- **Fuites de Données :** Sans les mesures de sécurité adéquates, les données sensibles peuvent être compromises.

### **2. Sécurité**

Pour minimiser les risques liés à l'exposition à Internet :

- **Instances Sensibles :** Placez les instances critiques ou contenant des données sensibles dans des sous-réseaux privés, sans accès direct à Internet.
- **Accès Externes :** Gérez les accès externes via un Bastion Host dans le sous-réseau public ou utilisez un VPN pour connecter les utilisateurs autorisés.

### **3. Cas d'Utilisation Courant**

- **Serveurs Web et Applications :** Les instances dans les sous-réseaux publics sont généralement des serveurs web ou des applications qui doivent être accessibles depuis l'extérieur. 
- **Backend et Bases de Données :** Placez les backend et bases de données dans des sous-réseaux privés, et n'autorisez l'accès qu'à partir de services autorisés.

## **Bonnes Pratiques**

1. **Limiter les Adresses IP Publiques :** N'attribuez des IP publiques qu'aux instances qui en ont absolument besoin.
2. **Configurer les Groupes de Sécurité :** Utilisez des règles strictes pour limiter les accès aux ports nécessaires uniquement.
3. **Mise en Place de la Surveillance :** Surveillez en permanence les instances pour détecter toute activité suspecte ou non autorisée.
4. **Utilisation du Bastion Host :** Limitez les connexions SSH ou RDP via un Bastion Host pour renforcer la sécurité des accès.

## **Conclusion**

En résumé, bien qu'il soit possible de créer une instance dans un sous-réseau public, ce n'est pas toujours recommandé, sauf si l'accès direct à Internet est nécessaire et que toutes les mesures de sécurité appropriées sont en place. Ce guide vise à vous fournir les connaissances nécessaires pour prendre des décisions éclairées concernant l'architecture de votre réseau et la sécurité de vos instances.

## **Références**

- [Documentation AWS sur les VPC](https://docs.aws.amazon.com/vpc/latest/userguide/what-is-amazon-vpc.html)
- [Meilleures Pratiques de Sécurité sur le Cloud](https://aws.amazon.com/architecture/security-identity-compliance/)

