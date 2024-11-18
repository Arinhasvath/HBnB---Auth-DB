### Introduction à la Partie 3 : Backend Amélioré avec Authentification et Intégration de Base de Données

Bienvenue dans la Partie 3 du **Projet HBnB**, où vous allez étendre le backend de l'application en introduisant **l'authentification des utilisateurs**, **l'autorisation**, et **l'intégration de base de données** en utilisant **SQLAlchemy** et **SQLite** pour le développement. Plus tard, vous configurerez **MySQL** pour les environnements de production. Dans cette partie, vous sécuriserez le backend, introduirez un stockage persistant, et préparerez l'application pour un déploiement évolutif et réel.

#### Objectifs du Projet
1. **Authentification et Autorisation** : Implémenter l'authentification des utilisateurs basée sur **JWT** en utilisant **Flask-JWT-Extended** et le contrôle d'accès basé sur les rôles avec l'attribut `is_admin` pour des points d'accès spécifiques.
2. **Intégration de Base de Données** : Remplacer le stockage en mémoire par **SQLite** pour le développement en utilisant **SQLAlchemy** comme ORM et préparer **MySQL** pour la production.
3. **Opérations CRUD avec Persistance en Base de Données** : Refactoriser toutes les opérations CRUD pour interagir avec une base de données persistante.
4. **Conception et Visualisation de Base de Données** : Concevoir le schéma de base de données en utilisant **mermaid.js** et s'assurer que toutes les relations entre les entités sont correctement mappées.
5. **Cohérence et Validation des Données** : S'assurer que la validation des données et les contraintes sont correctement appliquées dans les modèles.

#### Objectifs d'Apprentissage
À la fin de cette partie, vous serez capable de :
- Implémenter **l'authentification JWT** pour sécuriser votre API et gérer les sessions utilisateur.
- Appliquer un **contrôle d'accès basé sur les rôles** pour restreindre l'accès en fonction des rôles des utilisateurs (utilisateurs réguliers vs administrateurs).
- Remplacer les dépôts en mémoire par une **couche de persistance basée sur SQLite** en utilisant **SQLAlchemy** pour le développement et configurer **MySQL** pour la production.
- Concevoir et visualiser un **schéma de base de données relationnelle** en utilisant **mermaid.js** pour gérer les relations entre utilisateurs, lieux, avis et commodités.
- Assurer que le backend est sécurisé, évolutif et fournit un stockage de données fiable pour les environnements de production.

#### Contexte du Projet
Dans les parties précédentes du projet, vous avez travaillé avec un stockage en mémoire, idéal pour le prototypage mais insuffisant pour les environnements de production. Dans la Partie 3, vous passerez à **SQLite**, une base de données relationnelle légère, pour le développement, tout en préparant le système pour **MySQL** en production. Cela vous donnera une expérience pratique avec des systèmes de bases de données du monde réel, permettant à votre application d'évoluer efficacement.

De plus, vous introduirez **l'authentification basée sur JWT** pour sécuriser l'API, garantissant que seuls les utilisateurs authentifiés peuvent interagir avec certains points d'accès. Vous implémenterez également un contrôle d'accès basé sur les rôles pour appliquer des restrictions basées sur les privilèges de l'utilisateur (utilisateurs réguliers vs administrateurs).

#### Ressources du Projet
Voici quelques ressources qui vous guideront à travers cette partie du projet :
- **Authentification JWT** : [Documentation Flask-JWT-Extended](https://flask-jwt-extended.readthedocs.io/en/stable/)
- **ORM SQLAlchemy** : [Documentation SQLAlchemy](https://docs.sqlalchemy.org/en/20/)
- **SQLite** : [Documentation SQLite](https://sqlite.org/docs.html)
- **MySQL** : [Documentation MySQL](https://dev.mysql.com/doc/)
- **Documentation Flask** : [Documentation Officielle Flask](https://flask.palletsprojects.com/en/2.0.x/)
- **Mermaid.js pour les Diagrammes ER** : [Documentation Mermaid.js](https://mermaid-js.github.io/mermaid/#/)

#### Structure du Projet
Dans cette partie du projet, les tâches sont organisées de manière à construire progressivement un système backend complet, sécurisé et basé sur une base de données :

1. **Modifier le Modèle Utilisateur pour Inclure le Mot de Passe** : Vous commencerez par modifier le modèle `User` pour stocker les mots de passe de manière sécurisée en utilisant bcrypt2 et mettre à jour la logique d'enregistrement des utilisateurs.
2. **Implémenter l'Authentification JWT** : Sécuriser l'API en utilisant des jetons JWT, garantissant que seuls les utilisateurs authentifiés peuvent accéder aux points d'accès protégés.
3. **Implémenter l'Autorisation pour des Points d'Accès Spécifiques** : Vous implémenterez un contrôle d'accès basé sur les rôles pour restreindre certaines actions (par exemple, actions réservées aux administrateurs).
4. **Intégration de la Base de Données SQLite** : Transition du stockage de données en mémoire vers **SQLite** comme base de données persistante pendant le développement.
5. **Mapper les Entités en Utilisant SQLAlchemy** : Mapper les entités existantes (`User`, `Place`, `Review`, `Amenity`) à la base de données en utilisant SQLAlchemy et s'assurer que les relations sont bien définies.
6. **Préparer MySQL pour la Production** : Vers la fin de cette phase, vous configurerez l'application pour utiliser **MySQL** en production et **SQLite** pour le développement.
7. **Conception et Visualisation de la Base de Données** : Utiliser **mermaid.js** pour créer des diagrammes entité-relation pour votre schéma de base de données.

Chaque tâche est soigneusement conçue pour s'appuyer sur le travail précédent et assurer une transition en douceur du système du développement à la préparation pour la production.

---

À la fin de la Partie 3, vous aurez un backend qui non seulement stocke les données dans une base de données persistante et sécurisée, mais garantit également que seuls les utilisateurs autorisés peuvent accéder et modifier des données spécifiques. Vous aurez implémenté des pratiques d'authentification et de gestion de base de données conformes aux normes de l'industrie, cruciales pour les applications web du monde réel.

Citations:
[1] https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/29334386/2eb483e9-1944-4a4e-b7d7-c71881e41634/paste.txt
[2] https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/29334386/8f3492d1-246c-42c6-90cd-51c2e91e1937/paste-2.txt
