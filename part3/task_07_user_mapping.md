### Tâche : Mapper l'entité Utilisateur vers un modèle SQLAlchemy

### Étapes

#### 1. Mapper la classe `BaseModel`

Créez un modèle de base dans `models/baseclass.py` pour gérer les attributs communs de toutes les entités.

```python
# models/baseclass.py

from app import db
import uuid
from datetime import datetime

class BaseModel(db.Model):
    __abstract__ = True  # SQLAlchemy ne crée pas de table pour cette classe

    id = db.Column(db.String(36), primary_key=True, default=lambda: str(uuid.uuid4()))
    created_at = db.Column(db.DateTime, default=datetime.utcnow)
    updated_at = db.Column(db.DateTime, default=datetime.utcnow, onupdate=datetime.utcnow)
```

#### 2. Mapper l'entité `User`

Définissez le modèle `User` dans `models/user.py`, héritant de `BaseModel`, et configurez les attributs spécifiques comme `first_name`, `email`, `password`, et `is_admin`.

```python
# models/user.py

from app import db, bcrypt
from .baseclass import BaseModel

class User(BaseModel):
    __tablename__ = 'users'

    first_name = db.Column(db.String(50), nullable=False)
    last_name = db.Column(db.String(50), nullable=False)
    email = db.Column(db.String(120), nullable=False, unique=True)
    password = db.Column(db.String(128), nullable=False)
    is_admin = db.Column(db.Boolean, default=False)

    def hash_password(self, password):
        """Hash du mot de passe avant stockage."""
        self.password = bcrypt.generate_password_hash(password).decode('utf-8')

    def verify_password(self, password):
        """Vérifie le mot de passe hashé."""
        return bcrypt.check_password_hash(self.password, password)
```

#### 3. Implémenter `UserRepository`

Créez une classe `UserRepository` spécifique pour gérer les opérations de base de données liées aux utilisateurs.

```python
# services/repositories/user_repository.py

from app.models.user import User
from app import db
from app.persistence.repository import SQLAlchemyRepository

class UserRepository(SQLAlchemyRepository):
    def __init__(self):
        super().__init__(User)

    def get_user_by_email(self, email):
        return self.model.query.filter_by(email=email).first()
```

#### 4. Mettre à jour le `HBnBFacade` pour utiliser `UserRepository`

Modifiez la classe `HBnBFacade` afin d'utiliser `UserRepository` pour les opérations sur les utilisateurs.

```python
# services/facade.py

from app.services.repositories.user_repository import UserRepository

class HBnBFacade:
    def __init__(self):
        self.user_repo = UserRepository()

    def create_user(self, user_data):
        user = User(**user_data)
        user.hash_password(user_data['password'])
        self.user_repo.add(user)
        return user

    def get_user(self, user_id):
        return self.user_repo.get(user_id)

    def get_user_by_email(self, email):
        return self.user_repo.get_user_by_email(email)
```

#### 5. Initialiser la base de données et tester l'intégration

Lancez la création de la base de données et testez l'application avec cURL ou Postman.

```bash
flask shell
>>> from app import db
>>> db.create_all()
```

**Créer un utilisateur :**
```bash
curl -X POST "http://127.0.0.1:5000/api/v1/users/" -H "Content-Type: application/json" -d '{
  "first_name": "John",
  "last_name": "Doe",
  "email": "john.doe@example.com",
  "password": "password123"
}'
```

**Récupérer un utilisateur par ID :**
```bash
curl -X GET "http://127.0.0.1:5000/api/v1/users/<user_id>"
```

--- 

En suivant ces étapes, le modèle `User` sera intégré dans la base de données, et l'API sera prête pour interagir avec les données utilisateurs.