### Tâche : Scripts SQL pour la génération des tables et des données initiales

#### Objectif
Créer des scripts SQL pour générer l'ensemble du schéma de base de données pour le projet HBnB et le peupler avec des données initiales. Les scripts doivent inclure toutes les tables et relations nécessaires pour refléter le modèle du projet et insérer les données initiales requises.

#### Contexte
Dans cette tâche, vous vous concentrerez sur la conception du schéma de base de données en utilisant du SQL brut pour générer des tables et insérer des données initiales. Cela comprend la création de tables pour `Utilisateur`, `Lieu`, `Avis`, `Commodité`, et leurs relations. Le but de cette tâche est de pratiquer la définition des bases de données indépendamment de tout ORM, en s'assurant que vous comprenez comment concevoir des tables et des relations au niveau SQL.

#### Instructions

1. **Générer des scripts SQL pour la création des tables**

   Définissez des scripts SQL pour chacune des tables suivantes :

   - **Table Utilisateur** :
     - `id` : `CHAR(36) PRIMARY KEY` (format UUID)
     - `prénom` : `VARCHAR(255)`
     - `nom` : `VARCHAR(255)`
     - `email` : `VARCHAR(255) UNIQUE`
     - `mot_de_passe` : `VARCHAR(255)`
     - `est_admin` : `BOOLEAN DEFAULT FALSE`
   
   - **Table Lieu** :
     - `id` : `CHAR(36) PRIMARY KEY` (format UUID)
     - `titre` : `VARCHAR(255)`
     - `description` : `TEXT`
     - `prix` : `DECIMAL(10, 2)`
     - `latitude` : `FLOAT`
     - `longitude` : `FLOAT`
     - `propriétaire_id` : `CHAR(36)` (Clé étrangère référençant `Utilisateur(id)`)

   - **Table Avis** :
     - `id` : `CHAR(36) PRIMARY KEY` (format UUID)
     - `texte` : `TEXT`
     - `note` : `INT` (Entre 1 et 5)
     - `utilisateur_id` : `CHAR(36)` (Clé étrangère référençant `Utilisateur(id)`)
     - `lieu_id` : `CHAR(36)` (Clé étrangère référençant `Lieu(id)`)
     - Ajouter une **contrainte unique** sur la combinaison de `utilisateur_id` et `lieu_id` pour s'assurer qu'un utilisateur ne peut laisser qu'un seul avis par lieu

   - **Table Commodité** :
     - `id` : `CHAR(36) PRIMARY KEY` (format UUID)
     - `nom` : `VARCHAR(255) UNIQUE`

   - **Table Lieu_Commodité** (Relation plusieurs-à-plusieurs) :
     - `lieu_id` : `CHAR(36)` (Clé étrangère référençant `Lieu(id)`)
     - `commodité_id` : `CHAR(36)` (Clé étrangère référençant `Commodité(id)`)
     - Ajouter une **clé primaire composite** pour `lieu_id` et `commodité_id`

   **Assurez-vous que** :
   - Les contraintes de clé étrangère sont correctement établies pour les relations
   - Les UUID sont correctement générés pour les champs `id`

2. **Insérer les données initiales**

   Insérez les données initiales dans la base de données en utilisant des instructions SQL `INSERT` :

   - **Utilisateur administrateur** :
     - `id` : `36c9050e-ddd3-4c3b-9731-9f487208bbc1` (Cet ID est fixe)
     - `email` : `admin@hbnb.io`
     - `prénom` : `Admin`
     - `nom` : `HBnB`
     - `mot_de_passe` : Hacher le mot de passe `admin1234` en utilisant bcrypt2 (utilisez un script ou un outil pour générer le hash)
     - `est_admin` : `True`

   - **Commodités initiales** :
     Insérez les commodités suivantes dans la table `Commodité` avec un UUID généré aléatoirement :
     - WiFi
     - Piscine
     - Climatisation

     Générez les valeurs `UUID4` pour chaque `id` en utilisant un générateur UUID en ligne ou un script Python.

3. **Tester les scripts SQL**

   - **Création des tables** : Assurez-vous que les tables sont créées avec succès, avec toutes les contraintes et relations en place
   - **Insertion des données** : Vérifiez que les données initiales sont insérées correctement, en vous assurant que le mot de passe est stocké au format haché
   
   **Tester les opérations CRUD** :
   - Utilisez les instructions `SELECT`, `INSERT`, `UPDATE` et `DELETE` pour tester l'intégrité des données et la fonctionnalité CRUD pour chaque table
   - Vérifiez que l'utilisateur administrateur est créé avec `est_admin = TRUE` et que les commodités sont insérées correctement

---

#### Ressources
- **Tutoriel SQL pour débutants** : [W3Schools SQL Tutorial](https://www.w3schools.com/sql/)

#### Résultat attendu
À la fin de cette tâche, vous devriez avoir :
- Des scripts SQL qui génèrent le schéma complet de la base de données
- Inséré un utilisateur administrateur et des commodités dans la base de données
- Testé les opérations CRUD pour vérifier le bon fonctionnement du schéma et des données