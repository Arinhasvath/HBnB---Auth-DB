### Implémentation du Repository SQLAlchemy

#### Objectif
Remplacer le dépôt en mémoire par un dépôt basé sur SQLAlchemy pour permettre la persistance des données. Dans cette tâche, vous allez créer le `SQLAlchemyRepository` et l'intégrer dans le projet pour gérer les interactions avec la base de données. Cette étape prépare l'application à l'utilisation d'une base de données relationnelle en environnement de production.

#### Contexte
Dans les parties précédentes du projet, la couche de persistance était gérée avec un dépôt en mémoire. Cette tâche introduit SQLAlchemy pour stocker les données dans une base de données SQLite durant le développement, en vue de rendre l'application prête pour une base de données relationnelle. Le modèle du dépôt reste le même, mais l'implémentation utilisera désormais SQLAlchemy pour toutes les opérations CRUD. La base de données n'étant pas encore initialisée, cette tâche se concentre uniquement sur la création du dépôt. Le mapping des modèles et l'initialisation de la base de données seront traités dans la prochaine tâche.

#### Instructions

1. **Configurer SQLAlchemy dans le Projet**

   - Ajoutez les dépendances nécessaires pour SQLAlchemy dans votre fichier `requirements.txt` :
     ```txt
     sqlalchemy
     flask-sqlalchemy
     ```

   - Initialisez SQLAlchemy dans `app/__init__.py`, sans encore procéder à l'initialisation de la base de données (la création des tables sera effectuée dans la tâche suivante) :
     ```python
     from flask import Flask
     from flask_sqlalchemy import SQLAlchemy

     db = SQLAlchemy()

     def create_app(config_class=config.DevelopmentConfig):
         #
         # Code existant pour créer une instance de Flask
         #
         db.init_app(app)

         # ...
     ```

2. **Ajouter les Paramètres de Configuration pour SQLAlchemy**

   - Ajoutez les clés de configuration suivantes dans la classe `DevelopmentConfig` du fichier `config.py` :
     ```python
     import os
     
     class Config:
         SECRET_KEY = os.getenv('SECRET_KEY', 'default_secret_key')
         DEBUG = False
     
     class DevelopmentConfig(Config):
         DEBUG = True
         SQLALCHEMY_DATABASE_URI = 'sqlite:///development.db'
         SQLALCHEMY_TRACK_MODIFICATIONS = False
     
     config = {
         'development': DevelopmentConfig,
         'default': DevelopmentConfig
     }
     ```

   - `SQLALCHEMY_DATABASE_URI`: Définit l'URI de la base de données SQLite.
   - `SQLALCHEMY_TRACK_MODIFICATIONS`: Désactive le suivi des modifications des objets, une fonctionnalité qui peut consommer de la mémoire supplémentaire.     

3. **Créer le Repository SQLAlchemy**

   - La classe `SQLAlchemyRepository` implémente l'interface `Repository` de **app/persistence/repository.py**, fournie dans les tâches précédentes du projet.
   - Ce dépôt gère les opérations CRUD de la base de données avec SQLAlchemy, en conservant la même structure que le dépôt en mémoire.

   ```python
   from app import db

   class SQLAlchemyRepository(Repository):
       def __init__(self, model):
           self.model = model

       def add(self, obj):
           db.session.add(obj)
           db.session.commit()

       def get(self, obj_id):
           return self.model.query.get(obj_id)

       def get_all(self):
           return self.model.query.all()

       def update(self, obj_id, data):
           obj = self.get(obj_id)
           if obj:
               for key, value in data.items():
                   setattr(obj, key, value)
               db.session.commit()

       def delete(self, obj_id):
           obj = self.get(obj_id)
           if obj:
               db.session.delete(obj)
               db.session.commit()

       def get_by_attribute(self, attr_name, attr_value):
           return self.model.query.filter_by(**{attr_name: attr_value}).first()
   ```

   - Ce `SQLAlchemyRepository` est flexible et réutilisable pour différentes entités du projet (ex. : `User`, `Place`, `Review`).

4. **Refactoriser la Façade**

   - La classe Façade doit désormais utiliser le `SQLAlchemyRepository` pour interagir avec la base de données. Puisque le mapping des modèles et l'initialisation de la base de données n'ont pas encore eu lieu, seule la structure sera mise en place pour l'instant.
   - Refactorisez le `HBnBFacade` dans `app/services/facade.py` pour remplacer le dépôt en mémoire par le dépôt basé sur SQLAlchemy :

     ```python
     from app.models.user import User
     from app.persistence.repository import SQLAlchemyRepository

     class HBnBFacade:
         def __init__(self):
             self.user_repo = SQLAlchemyRepository(User)

         def create_user(self, user_data):
             user = User(**user_data)
             self.user_repo.add(user)
             return user

         def get_user(self, user_id):
             return self.user_repo.get(user_id)
     ```

5. **Reporter les Tests de la Base de Données**
   - **Important** : Les modèles n'ayant pas encore été mappés, il est impossible de tester ou d'initialiser complètement la base de données à ce stade. Dans la prochaine tâche, le modèle `User` sera mappé à la base de données, ce qui permettra de créer les tables et d'initialiser le schéma de la base de données.
   - Les tests et la vérification de la couche de persistance seront couverts une fois le mapping des modèles effectué.

---

#### Ressources

- **Documentation SQLAlchemy** : [SQLAlchemy](https://docs.sqlalchemy.org/en/20/)
- **Documentation Flask-SQLAlchemy** : [Flask-SQLAlchemy](https://flask-sqlalchemy.palletsprojects.com/en/2.x/)

---

#### Résultat Attendu
À la fin de cette tâche, vous aurez implémenté le `SQLAlchemyRepository` et refactorisé la Façade pour utiliser ce nouveau dépôt pour la persistance des données. Cependant, étant donné que le mapping des modèles et l'initialisation de la base de données seront effectués lors de la prochaine étape, les tests d'intégration seront reportés jusqu'à la prochaine tâche.