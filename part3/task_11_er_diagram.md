### Tâche : Générer des diagrammes de base de données

#### Objectif
Créer des diagrammes Entité-Relation (ER) pour représenter visuellement la structure du schéma de base de données du projet HBnB en utilisant **Mermaid.js**. Cette tâche aidera à s'assurer que le schéma de base de données est exactement reflété et cohérent avec les parties précédentes du projet. Les étudiants utiliseront Mermaid.js pour visualiser les tables et leurs relations dans un format lisible.

#### Contexte
La visualisation des relations de base de données est cruciale pour comprendre les connexions entre les entités. Les diagrammes ER fournissent une vue d'ensemble de la structure de la base de données et servent de référence essentielle pour le développement ou le débogage ultérieur. Mermaid.js est un outil qui permet de créer ces diagrammes en syntaxe de type markdown, ce qui facilite leur intégration dans des plateformes de documentation et de collaboration comme GitHub ou GitLab.

Dans cette tâche, les étudiants vont :
1. Apprendre à utiliser Mermaid.js pour créer des diagrammes ER.
2. Générer des diagrammes qui représentent les tables `User`, `Place`, `Review`, `Amenity`, et `Place_Amenity`, ainsi que leurs relations.
3. Assurer la cohérence de la visualisation du schéma, en utilisant Mermaid.js comme principal outil pour la génération des diagrammes.

---

### Instructions

1. **Apprendre la syntaxe Mermaid.js pour les diagrammes ER**

   Mermaid.js permet la génération de diagrammes via une syntaxe textuelle simple. Dans ce cas, nous nous concentrerons sur la création de **diagrammes ER** pour visualiser la structure d'une base de données relationnelle.

   Voici un exemple général de syntaxe Mermaid.js pour un diagramme ER :

   ```mermaid
   erDiagram
       PERSON {
           int id
           string first_name
           string last_name
           date birth_date
       }

       ADDRESS {
           int id
           string street
           string city
           string postal_code
       }

       COMPANY {
           int id
           string name
       }

       PERSON ||--o{ ADDRESS : has
       COMPANY ||--o{ PERSON : employs
   ```

   **Explication** :
   - `PERSON`, `ADDRESS`, et `COMPANY` sont des entités (tables) dans la base de données.
   - La notation `||--o{` est utilisée pour représenter les relations :
     - `||` signifie une relation un-à-un.
     - `o{` signifie une relation un-à-plusieurs.
   - Les attributs à l'intérieur de chaque entité représentent les colonnes de la table, avec des types comme `int`, `string`, et `date`.

2. **Générer le diagramme de la base de données pour HBnB**

   Après avoir appris la syntaxe, créez un diagramme ER représentant les entités principales de votre projet HBnB : `User`, `Place`, `Review`, `Amenity`, et `Place_Amenity`.

   **Entités à inclure** :

   - **User**
     - Attributs : `id`, `first_name`, `last_name`, `email`, `password`, `is_admin`

   - **Place**
     - Attributs : `id`, `title`, `description`, `price`, `latitude`, `longitude`, `owner_id`
     - Relations : `Place` a une clé étrangère `owner_id` référençant la table `User`.

   - **Review**
     - Attributs : `id`, `text`, `rating`, `user_id`, `place_id`
     - Relations : `Review` a des clés étrangères `user_id` (référence `User`) et `place_id` (référence `Place`).

   - **Amenity**
     - Attributs : `id`, `name`

   - **Place_Amenity** (Une table de jointure pour représenter la relation plusieurs-à-plusieurs entre `Place` et `Amenity`)
     - Attributs : `place_id`, `amenity_id`
     - Relations :
       - `Place_Amenity` a deux clés étrangères :
         - `place_id` (référence `Place`)
         - `amenity_id` (référence `Amenity`)

3. **Générer et exporter le diagramme**

   Après avoir écrit le code Mermaid.js pour le diagramme ER :

   - Utilisez un éditeur en ligne comme [Mermaid Live Editor](https://mermaid-js.github.io/mermaid-live-editor/) pour visualiser votre diagramme.
   - Exportez le diagramme en tant qu'image (par exemple, PNG ou SVG) à des fins de documentation.

   Vous pouvez également intégrer des diagrammes Mermaid dans des plateformes comme GitHub en incluant le code du diagramme directement dans vos fichiers markdown.

4. **Tester votre compréhension**

   Pour vous assurer que vous comprenez comment fonctionnent les relations, modifiez le diagramme pour inclure de nouvelles entités potentielles, comme une table `Reservation` ou `Booking`, et comment elles pourraient être liées à `User` et `Place`.

---

#### Ressources

- **Documentation Mermaid.js** : [Mermaid.js Official Docs](https://mermaid-js.github.io/mermaid/)
- **Éditeur en direct Mermaid.js** : [Mermaid Live Editor](https://mermaid-js.github.io/mermaid-live-editor/)
- **Documentation des relations SQLAlchemy** : [SQLAlchemy ORM Relationships](https://docs.sqlalchemy.org/en/20/orm/basic_relationships.html)

---

#### Résultat attendu

À la fin de cette tâche, vous devriez avoir :

- Créé un diagramme ER dans Mermaid.js qui représente avec précision le schéma de base de données pour HBnB.
- Compris les différents types de relations (un-à-plusieurs, plusieurs-à-plusieurs) et comment elles sont représentées visuellement.
- Exporté le diagramme pour l'inclure dans la documentation de votre projet.