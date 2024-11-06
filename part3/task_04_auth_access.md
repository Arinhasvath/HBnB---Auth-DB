### Tâche : Implémenter des Endpoints d'Accès Utilisateur Authentifié

#### Objectif
Sécuriser divers endpoints de l'API pour permettre uniquement aux utilisateurs authentifiés d'effectuer des actions spécifiques, telles que la création et la modification de lieux et d'avis, ainsi que la mise à jour de leurs propres détails utilisateur. L'accès sera contrôlé via l'authentification JWT, avec une validation supplémentaire pour garantir que les utilisateurs ne peuvent modifier que leurs propres données (par exemple, les lieux qu'ils possèdent, les avis qu'ils ont créés).

#### Contexte
L'accès utilisateur authentifié est une partie essentielle de la sécurisation d'une API. En s'assurant que seuls les utilisateurs autorisés peuvent effectuer certaines actions, l'intégrité des données est protégée. Cette tâche se concentre sur la sécurisation des endpoints liés à la création et la modification de lieux et d'avis, tout en permettant aux utilisateurs de modifier leurs propres données.

Dans cette tâche, vous allez :
1. Sécuriser les endpoints pour garantir que seuls les utilisateurs authentifiés peuvent créer, mettre à jour et supprimer des ressources.
2. Ajouter une logique pour valider la propriété des lieux et des avis.
3. Implémenter une logique pour empêcher les utilisateurs de commenter leurs propres lieux ou de commenter un lieu plusieurs fois.
4. Vérifier que les utilisateurs publics peuvent accéder aux endpoints **PUBLICS** sans un jeton JWT.

---

#### Instructions

1. **Sécuriser les Endpoints avec l'Authentification JWT**
   Les endpoints suivants seront protégés par l'authentification JWT, et une validation supplémentaire sera ajoutée pour garantir que les utilisateurs ne peuvent modifier que leurs propres données :

   - **POST /api/v1/places/** : Créer un nouveau lieu.
   - **PUT /api/v1/places/<place_id>** : Mettre à jour les détails d'un lieu. Seul le propriétaire du lieu peut modifier ses informations.
   - **POST /api/v1/reviews/** : Créer un nouvel avis. Les utilisateurs ne peuvent commenter que les lieux qu'ils **ne possèdent pas** et ne peuvent créer **qu'un seul** avis par lieu.
   - **PUT /api/v1/reviews/<review_id>** : Mettre à jour un avis. Les utilisateurs ne peuvent modifier que les avis qu'ils ont créés.
   - **DELETE /api/v1/reviews/<review_id>** : Supprimer un avis. Les utilisateurs ne peuvent supprimer que les avis qu'ils ont créés.
   - **PUT /api/v1/users/<user_id>** : Modifier les informations utilisateur. Les utilisateurs ne peuvent modifier que leurs propres détails (à l'exclusion de l'email et du mot de passe).

2. **Identifier les Endpoints Publics**
   - Les endpoints suivants seront accessibles publiquement :
     - **GET** `/api/v1/places/` : Récupérer une liste de lieux disponibles.
     - **GET** `/api/v1/places/<place_id>` : Récupérer des informations détaillées sur un lieu spécifique.

3. **Implémenter la Logique pour l'Authentification JWT**
   Utilisez le décorateur `@jwt_required()` de `flask-jwt-extended` pour appliquer l'authentification JWT sur les endpoints ci-dessus. La fonction `get_jwt_identity()` sera utilisée pour identifier l'utilisateur actuellement connecté.

   **Example:**
   ```python
   from flask_restx import Namespace, Resource, fields
   from flask_jwt_extended import jwt_required, get_jwt_identity

   api = Namespace('places', description='Place operations')

   @api.route('/')
   class PlaceList(Resource):
       @jwt_required()
       def post(self):
           """Create a new place"""
           current_user = get_jwt_identity()
           # Logic to create a new place for the logged-in user
           pass
   ```
4. **Logique de Validation**

   - **Créer un Lieu (POST /api/v1/places/)** :
     - S'assurer que l'utilisateur est authentifié.
     - L'ID du propriétaire (`owner_id`) du lieu doit être défini sur l'ID de l'utilisateur authentifié (récupéré en utilisant `get_jwt_identity()`).

    **Example:**
    ```python
    @api.route('/<place_id>')
    class PlaceResource(Resource):
        @jwt_required()
        def put(self, place_id):
            current_user = get_jwt_identity()
            place = facade.get_place(place_id)
            if place.owner_id != current_user:
                return {'error': 'Unauthorized action'}, 403
            # Logic to update the place
            pass
    ```
   - **Mettre à Jour un Lieu (PUT /api/v1/places/<place_id>)** :
     - S'assurer que l'utilisateur est authentifié.
     - Vérifier que l'`owner_id` du lieu correspond à l'ID de l'utilisateur authentifié.
     - Si l'utilisateur n'est pas le propriétaire, retourner une erreur 403 avec le message "Action non autorisée."

   - **Créer un Avis (POST /api/v1/reviews/)** :
     - S'assurer que l'utilisateur est authentifié.
     - Vérifier que l'`place_id` dans la requête appartient à un lieu que l'utilisateur **ne** possède **pas**.
     - Vérifier que l'utilisateur n'a pas déjà commenté ce lieu.
     - Si l'utilisateur est le propriétaire du lieu, retourner une erreur 400 avec le message "Vous ne pouvez pas commenter votre propre lieu."
     - Si l'utilisateur a déjà commenté le lieu, retourner une erreur 400 avec le message "Vous avez déjà commenté ce lieu."

   - **Mettre à Jour un Avis (PUT /api/v1/reviews/<review_id>)** :
     - S'assurer que l'utilisateur est authentifié.
     - Vérifier que l'`user_id` de l'avis correspond à l'utilisateur authentifié.
     - Si l'utilisateur n'a pas créé l'avis, retourner une erreur 403 avec le message "Action non autorisée."

   - **Supprimer un Avis (DELETE /api/v1/reviews/<review_id>)** :
     - S'assurer que l'utilisateur est authentifié.
     - Vérifier que l'`user_id` de l'avis correspond à l'utilisateur authentifié.
     - Si l'utilisateur n'a pas créé l'avis, retourner une erreur 403 avec le message "Action non autorisée."

   - **Modifier les Informations Utilisateur (PUT /api/v1/users/<user_id>)** :
     - S'assurer que l'utilisateur est authentifié.
     - Vérifier que l'`user_id` dans l'URL correspond à l'utilisateur authentifié.
     - Empêcher l'utilisateur de modifier son `email` et son `mot de passe` dans cet endpoint.
     - Si l'utilisateur tente de modifier son email ou son mot de passe, retourner une erreur 400 avec le message "Vous ne pouvez pas modifier l'email ou le mot de passe."
     - Si l'utilisateur tente de modifier les données d'un autre utilisateur, retourner une erreur 403 avec le message "Action non autorisée."

---

5. **Tester les Endpoints Authentifiés**

   Utilisez Postman ou cURL pour tester ces endpoints authentifiés. Assurez-vous que les actions non autorisées (par exemple, la modification d'un lieu que l'utilisateur ne possède pas) retournent les messages d'erreur appropriés.

   **Test Place Creation (POST /api/v1/places/):**
   ```bash
   curl -X POST "http://127.0.0.1:5000/api/v1/places/" -d '{"title": "New Place"}' -H "Authorization: Bearer <your_token>" -H "Content-Type: application/json"
   ```

   **Test Unauthorized Place Update (PUT /api/v1/places/<place_id>):**
   ```bash
   curl -X PUT "http://127.0.0.1:5000/api/v1/places/<place_id>" -d '{"title": "Updated Place"}' -H "Authorization: Bearer <your_token>" -H "Content-Type: application/json"
   ```

   **Expected Response for Unauthorized Action:**
   ```json
   {
       "error": "Unauthorized action"
   }
   ```

   **Test Creating a Review (POST /api/v1/reviews/):**
   ```bash
   curl -X POST "http://127.0.0.1:5000/api/v1/reviews/" -d '{"place_id": "<place_id>", "text": "Great place!"}' -H "Authorization: Bearer <your_token>" -H "Content-Type: application/json"
   ```

   **Test Updating a Review (PUT /api/v1/reviews/<review_id>):**
   ```bash
   curl -X PUT "http://127.0.0.1:5000/api/v1/reviews/<review_id>" -d '{"text": "Updated review"}' -H "Authorization: Bearer <your_token>" -H "Content-Type: application/json"
   ```

   **Test Deleting a Review (DELETE /api/v1/reviews/<review_id>):**
   ```bash
   curl -X DELETE "http://127.0.0.1:5000/api/v1/reviews/<review_id>" -H "Authorization: Bearer <your_token>"
   ```

   **Test Modifying User Data (PUT /api/v1/users/<user_id>):**
   ```bash
   curl -X PUT "http://127.0.0.1:5000/api/v1/users/<user_id>" -d '{"first_name": "Updated Name"}' -H "Authorization: Bearer <your_token>" -H "Content-Type: application/json"
   ```

6. **Tester les Endpoints Publics**
   - Testez les endpoints pour vérifier qu'ils sont accessibles sans un jeton JWT.

   **Example Test Using cURL:**
   - Retrieve a list of places:
     ```bash
     curl -X GET "http://127.0.0.1:5000/api/v1/places/"
     ```

   **Expected Response:**
   ```json
   [
       {
           "id": "1fa85f64-5717-4562-b3fc-2c963f66afa6",
           "title": "Cozy Apartment",
           "price": 100.0
       },
       {
           "id": "2fa85f64-5717-4562-b3fc-2c963f66afa6",
           "title": "Luxury Condo",
           "price": 200.0
       }
   ]
   ```

   - Retrieve detailed information about a specific place:
     ```bash
     curl -X GET "http://127.0.0.1:5000/api/v1/places/1fa85f64-5717-4562-b3fc-2c963f66afa6"
     ```

   **Expected Response:**
   ```json
   {
       "id": "1fa85f64-5717-4562-b3fc-2c963f66afa6",
       "title": "Cozy Apartment",
       "description": "A comfortable and affordable place to stay.",
       "price": 100.0,
       "latitude": 37.7749,
       "longitude": -122.4194
   }
   ```
---

#### Ressources

1. **Documentation Flask-JWT-Extended :** [Flask-JWT-Extended](https://flask-jwt-extended.readthedocs.io/en/stable/)
2. **Tester les API REST avec cURL :** [Everything cURL](https://everything.curl.dev/)

---

#### Résultat Attendu

À la fin de cette tâche, les endpoints suivants seront sécurisés, permettant uniquement aux utilisateurs authentifiés d'effectuer des actions en fonction de leur propriété des lieux et des avis :
- Créer, mettre à jour et supprimer des lieux (avec vérification de propriété).
- Créer et mettre à jour des avis (avec des restrictions sur la critique des lieux possédés et les avis en double).
- Modifier les détails utilisateur (à l'exclusion de l'email et du mot de passe).
