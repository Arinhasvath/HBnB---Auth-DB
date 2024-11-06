

**Tâche : Cartographier les relations entre les entités en utilisant SQLAlchemy**

#### Objetif
Cartographier les relations entre les entités en utilisant SQLAlchemy. Vous définirez à la fois les relations un-à-plusieurs et plusieurs-à-plusieurs et appliquerez les contraintes et les clés étrangères appropriées dans les modèles. Cette tâche servira de base pour relier les données liées dans votre application.

#### Contexte
Maintenant que vous avez défini les entités de base comme `Utilisateur`, `Lieu`, `Avis` et `Commodité`, il est temps d'établir des relations entre ces entités. Ces relations reflètent les connexions entre les concepts réels représentés par les entités (par exemple, un `Utilisateur` peut posséder plusieurs `Lieux`, un `Lieu` peut avoir plusieurs `Avis`). Définir des relations dans la base de données garantit que les données liées peuvent être facilement interrogées et manipulées de manière structurée.

Dans cette tâche, vous définirez des relations entre les entités en utilisant les capacités d'ORM de SQLAlchemy. Les relations comme "un-à-plusieurs" et "plusieurs-à-plusieurs" permettent une organisation claire et efficace des données, en respectant la structure logique de la base de données.

**Attributs de relation SQLAlchemy de base :**

- **`backref`** : Cette option crée une relation inverse automatiquement. Elle ajoute une nouvelle propriété à la table référencée, permettant de facilement parcourir la relation dans les deux sens.
- **`lazy`** : Cette option détermine quand SQLAlchemy charge les données liées. Les options courantes incluent :
  - `select` : Charge les données liées lorsque l'objet parent est accédé.
  - `subquery` : Charge tous les objets liés en utilisant une requête séparée lorsque l'objet parent est accédé.

---

##### **1. Relations un-à-plusieurs**

Dans une relation un-à-plusieurs, un enregistrement dans une table peut avoir plusieurs enregistrements associés dans une autre table. Par exemple, un client peut passer plusieurs commandes, mais chaque commande est associée à un seul client.

**Exemple :**
```python
from sqlalchemy import Column, Integer, String, ForeignKey
from sqlalchemy.orm import relationship
from app import db

class Parent(db.Model):
    __tablename__ = 'parents'
    id = Column(Integer, primary_key=True)
    enfants = relationship('Enfant', backref='parent', lazy=True)

class Enfant(db.Model):
    __tablename__ = 'enfants'
    id = Column(Integer, primary_key=True)
    parent_id = Column(Integer, ForeignKey('parents.id'), nullable=False)
```

**Explication :**
- Un `Parent` peut avoir plusieurs `Enfant` objects, mais chaque `Enfant` a un seul `Parent`.
- L'option `backref` dans `relationship()` crée un lien bidirectionnel, vous permettant d'accéder à tous les enfants d'un parent en utilisant `parent.enfants`.
- L'option `lazy=True` garantit que les objets `Enfant` liés sont chargés lorsque vous y accédez, vous permettant d'interroger et de travailler avec les données liées facilement.

---

##### **2. Relations plusieurs-à-plusieurs**

Dans une relation plusieurs-à-plusieurs, plusieurs enregistrements dans une table peuvent être liés à plusieurs enregistrements dans une autre table. Par exemple, un étudiant peut s'inscrire à plusieurs cours, et un cours peut avoir plusieurs étudiants inscrits. Comme vous le savez déjà, cela ne peut pas être modélisé directement dans une base de données relationnelle sans [ajouter une table supplémentaire](https://help.claris.com/en/pro-help/content/many-to-many-relationships.html). Pour que SQLAlchemy puisse relier ces données, la [table d'association doit également être mappée](https://docs.sqlalchemy.org/en/20/orm/basic_relationships.html#many-to-many).

**Exemple :**
```python
from sqlalchemy import Table, Column, Integer, ForeignKey
from sqlalchemy.orm import relationship
from app import db

# Table d'association pour la relation plusieurs-à-plusieurs
étudiant_cours = db.Table('étudiant_cours',
    Column('étudiant_id', Integer, ForeignKey('étudiants.id'), primary_key=True),
    Column('cours_id', Integer, ForeignKey('cours.id'), primary_key=True)
)

class Étudiant(db.Model):
    __tablename__ = 'étudiants'
    id = Column(Integer, primary_key=True)
    nom = Column(String, nullable=False)
    cours = relationship('Cours', secondary=étudiant_cours, lazy='subquery',
                           backref=db.backref('étudiants', lazy=True))

class Cours(db.Model):
    __tablename__ = 'cours'
    id = Column(Integer, primary_key=True)
    nom_cours = Column(String, nullable=False)
```

**Explication :**
- La table `étudiant_cours` est une table d'association qui lie `Étudiant` et `Cours`. Chaque entrée dans la table représente une inscription d'un étudiant dans un cours.
- Dans la classe `Étudiant`, la fonction `relationship()` utilise l'option `secondary` pour pointer vers la table d'association `étudiant_cours`, créant ainsi la relation plusieurs-à-plusieurs.
- L'option `backref` dans cet exemple permet d'accéder depuis `Cours` à ses étudiants associés en utilisant `cours.étudiants`.

---

#### Instructions

Maintenant que vous comprenez comment SQLAlchemy gère les relations, vous allez cartographier les relations entre les entités telles que `Utilisateur`, `Lieu`, `Avis` et `Commodité` dans votre projet.

##### **1. Relations à implémenter**

Vous devez implémenter les relations suivantes dans vos modèles :

- **Utilisateur et Lieu (un-à-plusieurs)** : Un `Utilisateur` peut créer plusieurs `Lieux`, mais chaque `Lieu` est associé à un seul `Utilisateur`.
- **Lieu et Avis (un-à-plusieurs)** : Un `Lieu` peut avoir plusieurs `Avis`, mais chaque `Avis` est associé à un seul `Lieu`.
- **Utilisateur et Avis (un-à-plusieurs)** : Un `Utilisateur` peut écrire plusieurs `Avis`, mais chaque `Avis` est écrit par un seul `Utilisateur`.
- **Lieu et Commodité (plusieurs-à-plusieurs)** : Un `Lieu` peut avoir plusieurs `Commodités`, et une `Commodité` peut être associée à plusieurs `Lieux`.

##### **2. Implémenter les relations dans vos entités**

En utilisant SQLAlchemy, vous définirez ces relations en ajoutant des clés étrangères et en utilisant `relationship()` pour créer des associations entre les modèles. Voici ce que vous devez faire pour chaque relation :

- **Utilisateur et Lieu** :  
  - Ajoutez une clé étrangère (`utilisateur_id`) dans le modèle `Lieu` pour référencer l'`Utilisateur`.
  - Utilisez `relationship()` dans les deux modèles pour les relier.

- **Lieu et Avis** :  
  - Ajoutez une clé étrangère (`lieu_id`) dans le modèle `Avis` pour référencer le `Lieu`.
  - Utilisez `relationship()` pour établir la relation un-à-plusieurs entre `Lieu` et `Avis`.

- **Utilisateur et Avis** :  
  - Ajoutez une clé étrangère (`utilisateur_id`) dans le modèle `Avis` pour référencer l'`Utilisateur`.
  - Utilisez `relationship()` pour relier l'`Utilisateur` et l'`Avis`.

- **Lieu et Commodité** :  
  - Créez une table d'association pour gérer la relation plusieurs-à-plusieurs entre `Lieu` et `Commodité`.
  - Utilisez `relationship()` dans les deux modèles `Lieu` et `Commodité` pour gérer la connexion via la table d'association.

##### **3. Tester les relations**

Après avoir défini les relations, vous devez les tester en utilisant Postman ou cURL. Assurez-vous de tester les deux extrémités des relations (par exemple, en récupérant les lieux d'un utilisateur, en obtenant les avis pour un lieu).

1. Initialisez la base de données avec `flask shell` :
   ```bash
   flask shell
   >>> from app import db
   >>> db.create_all()
   ```

2. Utilisez Postman ou cURL pour créer et relier des entités telles que des lieux, des avis et des commodités, en vérifiant les relations

---

#### Ressources
1. **Documentation SQLAlchemy :** [SQLAlchemy](https://docs.sqlalchemy.org/en/14/)
2. **Documentation Flask-SQLAlchemy :** [Flask-SQLAlchemy](https://flask-sqlalchemy.palletsprojects.com/en/2.x/)
3. **Modèles de relations SQLAlchemy :** [SQLAlchemy ORM Relationships](https://docs.sqlalchemy.org/en/20/orm/relationships.html)

---

#### Résultat attendu

À la fin de cette tâche, vous aurez réussi à cartographier les relations entre les entités en utilisant SQLAlchemy. Ces relations garantiront l'intégrité des données et permettront une interrogation efficace des données liées, permettant des opérations complexes telles que la récupération de tous les avis pour un lieu ou de tous les lieux possédés par un utilisateur. L'utilisation correcte de `relationship()`, `backref` et de clés étrangères établira des liens clairs et bidirectionnels entre vos entités.