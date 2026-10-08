# Livrable 1 — Fiche du projet

## 1.1 Description du sujet

**Domaine métier :** Le projet s'inscrit dans le domaine de la vente et l’exposition d'œuvres d'art en ligne.

**Problème traité :** Le projet consiste à mettre en place une plateforme permettant aux utilisateurs de publier et consulter des œuvres d'art. La plateforme doit gérer le cycle de vente des œuvres, notamment l'authentification des utilisateurs, l'exposition des œuvres, les notifications et la finalisation des ventes (achat direct à prix fixe).

**Application servant de charge de travail :** Application développée. Elle sera basée sur la stack **PERN** (PostgreSQL, Express, React et Node.js), la base de données MongoDB de la stack MERN classique étant remplacée par PostgreSQL (Amazon RDS) conformément aux services AWS imposés. La gestion des enchères en temps réel est écartée de la première version et pourra être ajoutée ultérieurement.

### Composants applicatifs
- **Frontend – React** : Interface web permettant aux utilisateurs de consulter les œuvres, publier des œuvres, rechercher et filtrer les œuvres, et acheter une œuvre.
- **Backend – Node.js / Express** : Fournit les API et contient la logique métier de l'application, notamment l'authentification, la gestion des œuvres, des commandes et des transactions de vente, ainsi que l'envoi des notifications.
- **Base de données – PostgreSQL (Amazon RDS)** : Assure le stockage des données de l'application, notamment les utilisateurs, les œuvres, les commandes et les informations relatives aux transactions de vente.

## 10.3 Objectifs non fonctionnels auto-imposés

| **Objectif**                                            | **Cible fixée par le groupe**                                                                  | **Mode de démonstration**                                                                                                                                                |
|---------------------------------------------------------|------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Tolérance à la perte d'une instance applicative**     | Perte d'une instance applicative **sans interruption de service**                              | Arrêter une instance du backend et vérifier que l'application reste accessible grâce à l'autre instance.                                                                 |
| **Charge nominale et charge de pointe visées**          | **50 utilisateurs concurrents** en charge nominale et **100 utilisateurs** en charge de pointe | Effectuer un test de charge **k6** et vérifier les temps de réponse et le taux d'erreur.                                                                                 |
| **Temps de reconstruction complet de l'infrastructure** | **≤ 15 minutes**                                                                               | Détruire puis reconstruire l'infrastructure à l'aide de **Terraform / Infrastructure as Code**, puis mesurer le temps nécessaire pour retrouver un service opérationnel. |
| **Perte de données maximale admissible**                | **≤ 5 minutes** de données                                                                     | Simuler une perte de données, restaurer la dernière sauvegarde et vérifier que la quantité maximale de données perdues ne dépasse pas 5 minutes.                         |
| **Budget total consommé sur le semestre**               | **≤ 50 USD**                                                                                  | Consulter le suivi des coûts de l'infrastructure Cloud et vérifier que la consommation reste inférieure ou égale au budget fixé.                                         |

## 10.4 Faisabilité préliminaire

### Services utilisés

> **VPC, sous-réseaux, tables de routage et Internet Gateway, Security Groups / NACL, EC2, EBS, ALB, Auto Scaling Group avec Launch Template, RDS PostgreSQL, S3, CloudWatch et IAM.**

Ces services seront utilisés pour mettre en place une architecture web 3-tiers résiliente, sécurisée et supervisée, avec React/Node.js en couche applicative, RDS PostgreSQL pour la persistance des données et S3 pour le stockage des fichiers.

### Solution d'accès

Les instances applicatives seront placées dans des sous-réseaux privés. Un mécanisme d'accès sortant contrôlé sera étudié afin de permettre aux instances d'accéder aux ressources externes nécessaires, tout en empêchant tout accès entrant direct depuis Internet. La solution retenue sera validée en fonction des services AWS autorisés et des contraintes du compte AWS Academy.

### Points de risque identifiés
- **Budget AWS limité** : risque de consommation excessive du crédit de 50 USD, notamment avec EC2, ALB et RDS.
- **Contraintes AWS Academy** : certaines fonctionnalités ou opérations peuvent être limitées par l'environnement de laboratoire.
- **Haute disponibilité** : risque de mauvaise configuration de l'ALB et de l'Auto Scaling sur les deux zones de disponibilité.
- **Accès à la base de données** : risque de mauvaise configuration des Security Groups entre les instances applicatives et RDS PostgreSQL.
- **Sauvegarde et restauration** : risque de perte de données en cas de mauvaise configuration des sauvegardes RDS.
- **Reconstruction IaC** : risque de dépendances ou de configurations manuelles empêchant une reconstruction complète de l'infrastructure.
- **Gestion des identifiants** : risque d'exposition des credentials AWS ou des secrets de l'application dans Git.
- **Accès sortant** : nécessité de trouver une solution compatible avec la liste des services autorisés et les contraintes du compte AWS Academy.
