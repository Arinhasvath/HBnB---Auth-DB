### Tâche : Modifier le modèle User pour inclure le hachage de mot de passe

#### Objectif
Mettre à jour le modèle `User` pour stocker de manière sécurisée un mot de passe haché en utilisant **bcrypt**. Modifier le point de terminaison d'enregistrement des utilisateurs pour accepter le champ `password` et s'assurer qu'il est haché avant d'être stocké. Le mot de passe ne doit **pas** être renvoyé dans les requêtes `GET`.

#### Contexte
Dans les tâches précédentes, le modèle `User` a été créé, mais il ne gérait pas les mots de passe. Dans cette tâche, vous allez améliorer le modèle `User` pour prendre en charge le hachage des mots de passe en utilisant **bcrypt**, assurant ainsi un stockage sécurisé des mots de passe. De plus, vous mettrez à jour le point de terminaison d'enregistrement des utilisateurs pour accepter et hacher les mots de passe avant de les stocker. Les mots de passe ne seront pas renvoyés dans les requêtes `GET`.

#### Instructions

1. **Installer le plugin `flask-bcrypt`**
   - Dans votre fichier `requirements.txt`, ajoutez la ligne suivante :
     ```
     flask-bcrypt
     ```
   - Exécutez la commande suivante pour l'installer :
     ```bash
     pip install flask-bcrypt
     ```

2. **Enregistrer le plugin dans l'application**
   - Dans le fichier `app/__init__.py` :
     - Importez `Bcrypt` du package `flask_bcrypt` et instanciez-le.
       
       ```python
       from flask_bcrypt import Bcrypt
      
       bcrypt = Bcrypt()
       ```
 
     - Initialisez l'instance dans la fonction `create_app()`.
       ```python
       def create_app(config_class=config.DevelopmentConfig):
          #
          # Code existant avec l'instance Flask app
          # ...
          bcrypt.init_app(app)
       ```

2. **Mettre à jour le modèle `User` pour inclure le hachage de mot de passe**
   - Dans le fichier `models/user.py` :
      - Mettez à jour la classe `User` pour inclure un attribut `password`. Ce champ stockera la version hachée du mot de passe de l'utilisateur.

   - Implémentez les méthodes suivantes dans la classe `User` :
     - **hash_password(password) :** Cette fonction doit prendre un mot de passe en texte clair, le hacher en utilisant **bcrypt**, et stocker la version hachée dans le champ `password`.
       
       ```python
       def hash_password(self, password):
           """Hache le mot de passe avant de le stocker."""
           self.password = bcrypt.generate_password_hash(password).decode('utf-8')
       ```
       
     - **verify_password(password) :** Cette fonction doit comparer un mot de passe en texte clair avec le mot de passe haché stocké dans le champ `password`, retournant `True` s'ils correspondent et `False` sinon.
       
       ```python
       def verify_password(self, password):
           """Vérifie si le mot de passe fourni correspond au mot de passe haché."""
           return bcrypt.check_password_hash(self.password, password)
       ```

3. **Modifier le point de terminaison d'enregistrement des utilisateurs (`POST /api/v1/users/`)**
   - Modifiez le point de terminaison d'enregistrement des utilisateurs pour accepter un champ `password` en plus du prénom, du nom et de l'email.
   - Assurez-vous que le `password` est haché en utilisant la fonction `hash_password` avant de stocker l'utilisateur dans le référentiel.
   - Après l'enregistrement de l'utilisateur, ne renvoyez **pas** le mot de passe dans la réponse. Vous ne renverrez que l'ID de l'utilisateur et un message de succès.

4. **Modifier le point de terminaison de récupération des utilisateurs (`GET /api/v1/users/<user_id>`)**
   - Mettez à jour le point de terminaison `GET` pour **exclure** le `password` de la réponse lors de la récupération des détails d'un utilisateur.

5. **Tester la logique de hachage des mots de passe**
   - Utilisez Postman ou cURL pour tester le point de terminaison POST /api/v1/users/, en vous assurant que le mot de passe est haché et n'est pas renvoyé dans la réponse.
   - Vérifiez que le point de terminaison GET /api/v1/users/<user_id> n'expose pas le champ du mot de passe.

#### Ressources
- **Documentation Flask-Bcrypt :** [Flask-Bcrypt](https://flask-bcrypt.readthedocs.io/en/latest/)
- **Meilleures pratiques de hachage des mots de passe :** [OWASP Password Storage Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Password_Storage_Cheat_Sheet.html)

#### Résultat attendu
À la fin de cette tâche, le modèle `User` hachera et stockera de manière sécurisée les mots de passe en utilisant **bcrypt**. Le point de terminaison `POST /api/v1/users/` acceptera les mots de passe et les hachera de manière sécurisée avant de les stocker. Les mots de passe ne seront pas renvoyés dans les requêtes `GET`.
