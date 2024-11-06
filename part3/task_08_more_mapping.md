### Tâche : Mapper les entités Place, Review et Amenity

#### Objectif
Mapper les entités `Place`, `Review`, et `Amenity` en modèles SQLAlchemy. Cette tâche nécessite d’appliquer les connaissances acquises lors du mapping de l’entité `User` dans la tâche précédente à ces nouvelles entités. Vous allez implémenter les attributs principaux de chaque entité, et dans les tâches suivantes, les relations entre les entités seront définies. Aucune relation entre ces entités ne doit être ajoutée pour le moment.

#### Contexte
Dans les tâches précédentes, vous avez mappé l’entité `User` et implémenté le `UserRepository` pour interagir avec la base de données via SQLAlchemy. Maintenant, vous allez étendre ce mapping aux entités `Place`, `Review` et `Amenity`. Ces mappings préparent la base pour les relations qui seront ajoutées plus tard entre les lieux, avis, commodités, et utilisateurs.

Dans cette tâche, vous allez :
1. Mapper les attributs principaux pour les entités `Place`, `Review` et `Amenity`.
2. Vous assurer que chaque entité dispose d’une fonctionnalité CRUD de base via le pattern repository.
3. Suivre le même processus utilisé pour mapper l’entité `User` dans les tâches précédentes.

---

#### Instructions

1. **Mapper les entités Place, Review et Amenity**

   - Utilisez SQLAlchemy pour mapper les attributs principaux des entités `Place`, `Review` et `Amenity`.
   - Pour chaque entité, définissez le modèle SQLAlchemy avec les attributs appropriés, en vous assurant d’attribuer des clés primaires et des contraintes pertinentes (par exemple, ``nullable=False`` pour les champs requis).

> [!WARNING]
> Ne **pas** inclure de relations entre les entités pour le moment (elles seront ajoutées plus tard).

   Les attributs pour chaque entité et leurs types de données attendus sont les suivants :
   - **Place :**
     - ``id`` (Integer) : Identifiant unique (clé primaire).
     - ``title`` (String) : Titre du lieu.
     - ``description`` (String) : Description du lieu.
     - ``price`` (Float) : Prix par nuit.
     - ``latitude`` (Float) : Latitude du lieu.
     - ``longitude`` (Float) : Longitude du lieu.
   
   - **Review :**
     - ``id`` (Integer) : Identifiant unique (clé primaire).
     - ``text`` (String) : Texte de l'avis.
     - ``rating`` (Integer) : Note (de 1 à 5).
   
   - **Amenity :**
     - ``id`` (Integer) : Identifiant unique (clé primaire).
     - ``name`` (String) : Nom de la commodité.

2. **Suivre les mêmes étapes que pour l'entité `User`**
   De la même manière que vous avez mappé l’entité `User` dans la tâche précédente, appliquez la même structure pour définir les modèles, repositories, et méthodes de façade pour `Place`, `Review`, et `Amenity`. Ces modèles doivent maintenant être persistés dans la base de données en utilisant SQLAlchemy.

> [!IMPORTANT] 
> Vous devez fournir des entités valides lors de l'appel de l'API pour respecter les règles de logique métier, mais elles ne doivent pas encore être mappées ou persistées.

3. **Tester les mappings des entités**
   - Initialisez la base de données en utilisant ``flask shell`` et ``db.create_all()`` après avoir défini vos modèles pour créer les tables correspondantes.
   - Utilisez Postman ou cURL pour tester les opérations CRUD (Create, Read, Update, Delete) pour chaque entité. Cela peut être fait de manière similaire à la façon dont vous avez testé l'entité `User`.

---

#### Ressources
1. **Documentation SQLAlchemy :** [SQLAlchemy](https://docs.sqlalchemy.org/en/20/)
2. **Documentation Flask-SQLAlchemy :** [Flask-SQLAlchemy](https://flask-sqlalchemy.palletsprojects.com/en/2.x/)
3. **Modèles de relations SQLAlchemy :** [SQLAlchemy ORM Relationships](https://docs.sqlalchemy.org/en/20/orm/relationships.html)

---

#### Résultat attendu
À la fin de cette tâche, vous devriez avoir mappé les entités `Place`, `Review`, et `Amenity` en modèles SQLAlchemy, en veillant à ce que leurs attributs de base soient stockés dans la base de données. Les repositories et méthodes de façade doivent être mis à jour pour gérer les opérations CRUD pour chaque entité. Aucune relation entre les entités ne doit être implémentée pour le moment ; cela sera fait dans une tâche ultérieure.
```