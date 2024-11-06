# HBnB---Auth-DB

# Partie 3 : Backend amélioré avec authentification et intégration de base de données

Bienvenue dans la Partie 3 du Projet HBnB, où vous allez étendre le backend de l'application en introduisant l'authentification des utilisateurs, l'autorisation et l'intégration de base de données en utilisant SQLAlchemy et SQLite pour le développement. Plus tard, vous configurerez MySQL pour les environnements de production. Dans cette partie, vous sécuriserez le backend, introduirez un stockage persistant et préparerez l'application pour un déploiement évolutif et réel.

## Objectifs du projet

- **Authentification et autorisation** : Implémenter l'authentification des utilisateurs basée sur JWT en utilisant Flask-JWT-Extended et le contrôle d'accès basé sur les rôles avec l'attribut is_admin pour des points de terminaison spécifiques.
- **Intégration de base de données** : Remplacer le stockage en mémoire par SQLite pour le développement en utilisant SQLAlchemy comme ORM et préparer pour MySQL ou d'autres SGBDR de qualité production.
- **Opérations CRUD avec persistance en base de données** : Refactoriser toutes les opérations CRUD pour interagir avec une base de données persistante.
- **Conception et visualisation de base de données** : Concevoir le schéma de base de données en utilisant mermaid.js et s'assurer que toutes les relations entre les entités sont correctement mappées.
- **Cohérence et validation des données** : S'assurer que la validation des données et les contraintes sont correctement appliquées dans les modèles.

## Objectifs d'apprentissage

À la fin de cette partie, vous serez capable de :

1. Implémenter l'authentification JWT pour sécuriser votre API et gérer les sessions utilisateur.
2. Appliquer le contrôle d'accès basé sur les rôles pour restreindre l'accès en fonction des rôles des utilisateurs (utilisateurs réguliers vs administrateurs).
3. Remplacer les référentiels en mémoire par une couche de persistance basée sur SQLite en utilisant SQLAlchemy pour le développement et configurer MySQL pour la production.
4. Concevoir et visualiser un schéma de base de données relationnelle en utilisant mermaid.js pour gérer les relations entre les utilisateurs, les lieux, les avis et les commodités.
5. Assurer que le backend est sécurisé, évolutif et fournit un stockage de données fiable pour les environnements de production.

## Contexte du projet

Dans les parties précédentes du projet, vous avez travaillé avec un stockage en mémoire, idéal pour le prototypage mais insuffisant pour les environnements de production. Dans la Partie 3, vous passerez à SQLite, une base de données relationnelle légère, pour le développement, tout en préparant le système pour MySQL en production. Cela vous donnera une expérience pratique avec des systèmes de bases de données du monde réel, permettant à votre application d'évoluer efficacement.

De plus, vous introduirez l'authentification basée sur JWT pour sécuriser l'API, garantissant que seuls les utilisateurs authentifiés peuvent interagir avec certains points de terminaison. Vous implémenterez également un contrôle d'accès basé sur les rôles pour appliquer des restrictions en fonction des privilèges de l'utilisateur (utilisateurs réguliers vs administrateurs).

## Ressources du projet

Voici quelques ressources qui vous guideront à travers cette partie du projet :

- Authentification JWT : [Documentation Flask-JWT-Extended](https://flask-jwt-extended.readthedocs.io/)
- ORM SQLAlchemy : [Documentation SQLAlchemy](https://docs.sqlalchemy.org/)
- SQLite : [Documentation SQLite](https://www.sqlite.org/docs.html)
- Documentation Flask : [Documentation officielle Flask](https://flask.palletsprojects.com/)
- Mermaid.js pour les diagrammes ER : [Documentation Mermaid.js](https://mermaid-js.github.io/mermaid/#/)

## Structure du projet

Dans cette partie du projet, les tâches sont organisées de manière à construire progressivement un système backend complet, sécurisé et basé sur une base de données :

1. **Modifier le modèle utilisateur pour inclure le mot de passe** : Vous commencerez par modifier le modèle User pour stocker les mots de passe de manière sécurisée en utilisant bcrypt2 et mettre à jour la logique d'enregistrement des utilisateurs.
2. **Implémenter l'authentification JWT** : Sécuriser l'API en utilisant des jetons JWT, garantissant que seuls les utilisateurs authentifiés peuvent accéder aux points de terminaison protégés.
3. **Implémenter l'autorisation pour des points de terminaison spécifiques** : Vous implémenterez le contrôle d'accès basé sur les rôles pour restreindre certaines actions (par exemple, actions réservées aux administrateurs).
4. **Intégration de la base de données SQLite** : Transition du stockage de données en mémoire vers SQLite comme base de données persistante pendant le développement.
5. **Mapper les entités en utilisant SQLAlchemy** : Mapper les entités existantes (User, Place, Review, Amenity) à la base de données en utilisant SQLAlchemy et s'assurer que les relations sont bien définies.
6. **Préparer pour MySQL en production** : Vers la fin de cette phase, vous configurerez l'application pour utiliser MySQL en production et SQLite pour le développement.
7. **Conception et visualisation de la base de données** : Utiliser mermaid.js pour créer des diagrammes entité-relation pour votre schéma de base de données.

Chaque tâche est soigneusement conçue pour s'appuyer sur le travail précédent et assurer que le système passe en douceur du développement à la préparation pour la production.

À la fin de la Partie 3, vous aurez un backend qui non seulement stocke les données dans une base de données persistante et sécurisée, mais garantit également que seuls les utilisateurs autorisés peuvent accéder et modifier des données spécifiques. Vous aurez implémenté des pratiques d'authentification et de gestion de base de données conformes aux normes de l'industrie, cruciales pour les applications web du monde réel.

Voici des tutoriels détaillés pour l'installation et la configuration de chacun des logiciels mentionnés, en format markdown, pour Windows et Linux :

# Logiciels

# Flask-JWT-Extended

## Installation

### Windows

1. Ouvrez une invite de commande ou PowerShell.
2. Assurez-vous que Python est installé en exécutant :
   ```
   python --version
   ```
3. Si Python n'est pas installé, téléchargez-le depuis [python.org](https://www.python.org/downloads/windows/).
4. Installez Flask-JWT-Extended avec pip :
   ```
   pip install flask-jwt-extended
   ```

### Linux

1. Ouvrez un terminal.
2. Vérifiez que Python est installé :
   ```
   python3 --version
   ```
3. Si Python n'est pas installé, utilisez le gestionnaire de paquets de votre distribution. Par exemple, sur Ubuntu :
   ```
   sudo apt update
   sudo apt install python3 python3-pip
   ```
4. Installez Flask-JWT-Extended :
   ```
   pip3 install flask-jwt-extended
   ```

## Configuration

1. Dans votre application Flask, importez les modules nécessaires :
   ```python
   from flask import Flask
   from flask_jwt_extended import JWTManager, create_access_token, jwt_required, get_jwt_identity
   ```

2. Initialisez l'application Flask et configurez JWT :
   ```python
   app = Flask(__name__)
   app.config['JWT_SECRET_KEY'] = 'votre_clé_secrète'  # Changez ceci!
   jwt = JWTManager(app)
   ```

3. Créez une route pour générer un token :
   ```python
   @app.route('/login', methods=['POST'])
   def login():
       username = request.json.get('username', None)
       password = request.json.get('password', None)
       if username != 'test' or password != 'test':
           return jsonify({"msg": "Bad username or password"}), 401
       
       access_token = create_access_token(identity=username)
       return jsonify(access_token=access_token)
   ```

4. Protégez une route avec JWT :
   ```python
   @app.route('/protected', methods=['GET'])
   @jwt_required()
   def protected():
       current_user = get_jwt_identity()
       return jsonify(logged_in_as=current_user), 200
   ```

# SQLAlchemy

## Installation

### Windows

1. Ouvrez une invite de commande ou PowerShell.
2. Installez SQLAlchemy avec pip :
   ```
   pip install sqlalchemy
   ```

### Linux

1. Ouvrez un terminal.
2. Installez SQLAlchemy :
   ```
   pip3 install sqlalchemy
   ```

## Configuration

1. Importez SQLAlchemy dans votre script Python :
   ```python
   from sqlalchemy import create_engine, Column, Integer, String
   from sqlalchemy.ext.declarative import declarative_base
   from sqlalchemy.orm import sessionmaker
   ```

2. Créez une connexion à la base de données :
   ```python
   engine = create_engine('sqlite:///example.db', echo=True)
   ```

3. Créez une classe de base pour vos modèles :
   ```python
   Base = declarative_base()
   ```

4. Définissez un modèle :
   ```python
   class User(Base):
       __tablename__ = 'users'
       id = Column(Integer, primary_key=True)
       name = Column(String)
       fullname = Column(String)
       nickname = Column(String)
   ```

5. Créez les tables dans la base de données :
   ```python
   Base.metadata.create_all(engine)
   ```

6. Créez une session pour interagir avec la base de données :
   ```python
   Session = sessionmaker(bind=engine)
   session = Session()
   ```

# SQLite

## Installation

### Windows

1. SQLite est généralement inclus avec Python sur Windows. Vérifiez en ouvrant une invite de commande et en tapant :
   ```
   python -c "import sqlite3; print(sqlite3.sqlite_version)"
   ```
2. Si vous avez besoin d'une version spécifique, téléchargez-la depuis [le site officiel de SQLite](https://www.sqlite.org/download.html).

### Linux

1. SQLite est souvent pré-installé sur Linux. Vérifiez en ouvrant un terminal et en tapant :
   ```
   sqlite3 --version
   ```
2. Si ce n'est pas le cas, installez-le avec le gestionnaire de paquets. Par exemple, sur Ubuntu :
   ```
   sudo apt update
   sudo apt install sqlite3
   ```

## Configuration

1. Pour utiliser SQLite dans Python, importez le module :
   ```python
   import sqlite3
   ```

2. Créez une connexion à une base de données (cela créera le fichier s'il n'existe pas) :
   ```python
   conn = sqlite3.connect('example.db')
   ```

3. Créez un curseur pour exécuter des commandes SQL :
   ```python
   c = conn.cursor()
   ```

4. Exécutez des commandes SQL :
   ```python
   c.execute('''CREATE TABLE stocks
                (date text, trans text, symbol text, qty real, price real)''')
   ```

5. Committez les changements et fermez la connexion :
   ```python
   conn.commit()
   conn.close()
   ```

# Flask

## Installation

### Windows

1. Ouvrez une invite de commande ou PowerShell.
2. Installez Flask avec pip :
   ```
   pip install flask
   ```

### Linux

1. Ouvrez un terminal.
2. Installez Flask :
   ```
   pip3 install flask
   ```

## Configuration

1. Créez un nouveau fichier Python (par exemple `app.py`) et importez Flask :
   ```python
   from flask import Flask
   ```

2. Créez une instance de l'application Flask :
   ```python
   app = Flask(__name__)
   ```

3. Définissez une route :
   ```python
   @app.route('/')
   def hello_world():
       return 'Hello, World!'
   ```

4. Exécutez l'application en mode debug :
   ```python
   if __name__ == '__main__':
       app.run(debug=True)
   ```

5. Pour lancer l'application, dans le terminal :
   ```
   python app.py
   ```

# Mermaid.js

Mermaid.js est une bibliothèque JavaScript, donc l'installation dépend de votre environnement de développement.

## Installation

### Utilisation via CDN

1. Ajoutez le script Mermaid dans votre HTML :
   ```html
   <script src="https://cdn.jsdelivr.net/npm/mermaid/dist/mermaid.min.js"></script>
   ```

### Installation via npm (pour les projets Node.js)

1. Ouvrez un terminal.
2. Naviguez vers le répertoire de votre projet.
3. Installez Mermaid :
   ```
   npm install mermaid
   ```

## Configuration

1. Initialisez Mermaid dans votre script JavaScript :
   ```javascript
   mermaid.initialize({startOnLoad:true});
   ```

2. Créez un diagramme dans votre HTML :
   ```html
   <div class="mermaid">
     graph TD
     A[Client] --> B[Load Balancer]
     B --> C[Server01]
     B --> D[Server02]
   </div>
   ```

3. Pour générer des diagrammes dynamiquement en JavaScript :
   ```javascript
   var element = document.querySelector("#graphDiv");
   var graphDefinition = 'graph TD; A-->B; B-->C; C-->D;';
   var insertSvg = function(svgCode, bindFunctions) {
     element.innerHTML = svgCode;
   };
   mermaid.render('graphDiv', graphDefinition, insertSvg);
   ```

Ces tutoriels devraient vous permettre d'installer et de configurer chacun de ces outils sur Windows et Linux. N'oubliez pas d'adapter les chemins et les commandes en fonction de votre environnement spécifique.

Citations:
[1] https://www.youtube.com/watch?v=vKuKp10LQEM
[2] https://www.tutorialspoint.com/sqlalchemy/index.htm
[3] https://www.youtube.com/watch?v=aX-ayOb_Aho
[4] https://www.freecodecamp.org/news/jwt-authentication-in-flask/
[5] https://www.sqlalchemy.org
[6] https://docs.sqlalchemy.org/en/20/orm/tutorial.html
[7] https://4geeks.com/lesson/what-is-jwt-and-how-to-implement-with-flask
[8] https://30dayscoding.com/blog/flask-jwt-extended-and-secure-authentication
