---
# Introduction au Load Balancer AWS
---

Le Load Balancer AWS est un service qui distribue automatiquement le trafic entrant de votre application sur plusieurs cibles, telles que des instances EC2, des conteneurs ou des adresses IP, dans une ou plusieurs zones de disponibilité. Le load balancing améliore la disponibilité et la tolérance aux pannes de votre application.

### Concepts Clés

1. **Load Balancer (équilibreur de charge)** : Un dispositif qui agit comme la porte d'entrée de votre application, en dirigeant le trafic entrant vers différentes cibles (comme des serveurs) selon des règles prédéfinies.

2. **Listener (écouteur)** : Un processus qui surveille les requêtes de connexion. Un écouteur est configuré pour écouter les connexions entrantes sur un port spécifique (par exemple, le port 80 pour HTTP) et les redirige vers des cibles en fonction des règles définies.

3. **Groupe de Cibles** : Un groupe de cibles (comme des instances EC2) qui reçoivent le trafic entrant selon les règles définies dans l'écouteur. Vous pouvez associer un ou plusieurs groupes de cibles à un écouteur.

4. **Vérifications de Santé** : Une fonctionnalité qui surveille la santé des cibles dans un groupe de cibles. Si une cible échoue aux vérifications de santé, le load balancer cesse de lui envoyer du trafic jusqu'à ce qu'elle se rétablisse.

### Types de Load Balancers AWS

AWS propose trois types de load balancers :

1. **Application Load Balancer (ALB)** : Idéal pour les applications web car il fonctionne au niveau applicatif (couche 7 du modèle OSI). Il peut router le trafic en fonction du contenu de la requête.

2. **Network Load Balancer (NLB)** : Idéal pour gérer des modèles de trafic volatils. Il fonctionne au niveau transport (couche 4) et peut gérer des millions de requêtes par seconde avec une latence ultra-faible.

3. **Classic Load Balancer (CLB)** : Un load balancer ancien qui fonctionne à la fois au niveau applicatif et au niveau transport.

### Tutoriel Étape par Étape

#### 1. Créer un Load Balancer

- **Étape 1 : Ouvrir la Console EC2**
  - Allez sur le [Tableau de Bord EC2](https://console.aws.amazon.com/ec2/).

- **Étape 2 : Choisir Load Balancers**
  - Dans le menu de gauche, sous "Load Balancing", cliquez sur "Load Balancers".

- **Étape 3 : Créer un Load Balancer**
  - Cliquez sur "Create Load Balancer".
  - Choisissez le type de load balancer dont vous avez besoin (Application, Network, ou Classic).

#### 2. Configurer l'Écouteur

- **Étape 1 : Ajouter un Écouteur**
  - Choisissez un protocole (HTTP, HTTPS, TCP, etc.) et un port (par exemple, 80 pour HTTP ou 443 pour HTTPS).
  
- **Étape 2 : Sélectionner des Groupes de Cibles**
  - Sélectionnez un groupe de cibles existant ou créez-en un nouveau.
  - Définissez le type de cible (Instance, IP, Lambda) et le protocole à utiliser dans le groupe de cibles.

#### 3. Configurer le Groupe de Cibles

- **Étape 1 : Définir le Groupe de Cibles**
  - Choisissez le nom du groupe de cibles, le protocole et le port.
  - Spécifiez le VPC dans lequel se trouvent vos cibles.

- **Étape 2 : Enregistrer les Cibles**
  - Ajoutez des instances ou des adresses IP à votre groupe de cibles.
  - Vous pouvez sélectionner manuellement des instances depuis votre tableau de bord EC2 ou fournir des adresses IP.

- **Étape 3 : Configurer les Vérifications de Santé**
  - Définissez les paramètres des vérifications de santé, comme le protocole, le chemin, le port, et la fréquence des vérifications.
  - Les vérifications de santé garantissent que le trafic n'est acheminé que vers des cibles en bonne santé.

#### 4. Réviser et Créer

- **Étape 1 : Réviser la Configuration**
  - Passez en revue tous vos paramètres, y compris les écouteurs, les groupes de cibles, et les vérifications de santé.

- **Étape 2 : Créer le Load Balancer**
  - Une fois satisfait de votre configuration, cliquez sur "Create Load Balancer".

- **Étape 3 : Tester Votre Load Balancer**
  - Accédez au nom DNS du load balancer depuis votre navigateur web pour vous assurer qu'il distribue correctement le trafic.

### Notes Finales

- **Groupes de Sécurité** : Assurez-vous que vos groupes de sécurité autorisent le trafic sur les ports utilisés par le load balancer et les cibles.
- **Certificats** : Si vous utilisez HTTPS, vous devrez configurer des certificats SSL/TLS via AWS Certificate Manager (ACM).

### Prochaines Étapes

1. **Surveillance** : Utilisez CloudWatch pour surveiller les performances et la santé de votre load balancer.
2. **Mise à l'Échelle** : Configurez des groupes d'auto-scaling pour ajuster automatiquement le nombre d'instances en fonction des modèles de trafic.

---
# Annexe : 
---


### Qu'est-ce qu'un Load Balancer AWS ?

Un Load Balancer AWS est un service qui distribue le trafic entrant d'applications ou de réseaux sur plusieurs cibles, comme des instances EC2, des conteneurs et des adresses IP, dans plusieurs zones de disponibilité. Cela garantit une haute disponibilité et fiabilité en dirigeant le trafic uniquement vers des cibles saines.

### Composants d'un Application Load Balancer

1. **Load Balancer** : Sert de point de contact unique pour les clients et distribue les requêtes entrantes vers plusieurs cibles.
2. **Écouteur (Listener)** : Un processus qui vérifie les demandes de connexion en utilisant le protocole et le port que vous configurez. Il a des règles qui déterminent comment les requêtes sont routées vers les cibles.
3. **Groupe de cibles (Target Group)** : Route les requêtes vers une ou plusieurs cibles enregistrées, telles que des instances EC2. Vous pouvez configurer des vérifications de santé pour vous assurer que le trafic est envoyé uniquement vers des cibles saines.

### Configuration d'un Application Load Balancer

#### Étape 1 : Configurer votre groupe de cibles

1. Ouvrez la console Amazon EC2.
2. Allez dans **Load Balancing** > **Groupes de cibles**.
3. Cliquez sur **Créer un groupe de cibles**.
4. Définissez le **Type de cible** sur **instance**.
5. Entrez un nom pour le groupe de cibles.
6. Gardez le protocole par défaut (HTTP) et le port (80).
7. Sélectionnez le VPC contenant vos instances.
8. Configurez les vérifications de santé et cliquez sur **Créer un groupe de cibles**.

#### Étape 2 : Créer un Application Load Balancer

1. Dans la console EC2, allez dans **Load Balancers**.
2. Cliquez sur **Créer un Load Balancer** et sélectionnez **Application Load Balancer**.
3. Fournissez un nom et sélectionnez le schéma (orienté Internet ou interne).
4. Choisissez au moins deux zones de disponibilité et les sous-réseaux publics correspondants.
5. Configurez les groupes de sécurité pour autoriser le trafic HTTP sur le port 80.
6. Définissez les écouteurs et les règles de routage.
7. Passez en revue et créez le load balancer.

#### Étape 3 : Tester votre Load Balancer

1. Après la création, vérifiez que le load balancer route le trafic vers les cibles enregistrées.
2. Dans la section **Load Balancers**, sélectionnez votre load balancer et copiez son nom DNS.
3. Collez le nom DNS dans un navigateur web pour vous assurer qu'il affiche la page par défaut de votre serveur.

### Ressources supplémentaires

- **Tutoriels et Vidéos** : Il existe des tutoriels vidéo qui fournissent des instructions visuelles sur la configuration des Load Balancers AWS.
- **Documentation AWS** : La documentation officielle d'AWS offre des instructions détaillées et des meilleures pratiques pour configurer différents types de load balancers.

En suivant ces étapes, vous pourrez configurer un Application Load Balancer AWS pour gérer et distribuer le trafic vers vos applications.
