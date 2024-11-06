### Implémentation des points de terminaison d'accès administrateur

#### Objectif
Restreindre l'accès à certains points de terminaison API pour que seuls les utilisateurs ayant des privilèges administrateur puissent effectuer certaines actions, telles que la création de nouveaux utilisateurs, la modification des détails de tout utilisateur (email et mot de passe inclus), ainsi que l'ajout ou la modification des commodités. Les administrateurs pourront également effectuer les mêmes tâches que les utilisateurs authentifiés, sans être limités par les restrictions de propriété sur les endroits (places) et les avis (reviews).

#### Instructions

1. **Définir les Permissions Administrateur**
   Les points de terminaison suivants seront restreints aux administrateurs uniquement :
   - ``POST /api/v1/users/`` : Créer un nouvel utilisateur.
   - ``PUT /api/v1/users/<user_id>`` : Modifier les détails d'un utilisateur, y compris l'email et le mot de passe.
   - ``POST /api/v1/amenities/`` : Ajouter une nouvelle commodité.
   - ``PUT /api/v1/amenities/<amenity_id>`` : Modifier les détails d'une commodité.

   Les administrateurs pourront également :
   - Modifier ou supprimer tout lieu ou avis, en contournant les restrictions de propriété qui s'appliquent aux utilisateurs réguliers.

2. **Ajouter le Contrôle d'Accès Basé sur les Rôles (RBAC)**
   Utilisez le décorateur ``@jwt_required()`` pour garantir que seuls les utilisateurs authentifiés puissent accéder à ces points de terminaison. La fonction ``get_jwt()`` permet de récupérer les informations du JWT, incluant le flag ``is_admin``.

   **Exemple pour vérifier le statut d'administrateur :**
   ```python
   from flask_restx import Namespace, Resource
   from flask_jwt_extended import jwt_required, get_jwt_identity, get_jwt

   api = Namespace('admin', description='Admin operations')

   @api.route('/users/<user_id>')
   class AdminUserResource(Resource):
       @jwt_required()
       def put(self, user_id):
           current_user = get_jwt_identity()
           
           if not current_user.get('is_admin'):
               return {'error': 'Admin privileges required'}, 403

           data = request.json
           email = data.get('email')

           if email:
               existing_user = facade.get_user_by_email(email)
               if existing_user and existing_user.id != user_id:
                   return {'error': 'Email is already in use'}, 400

           pass
   ```

3. **Implémenter la Logique des Points de Terminaison**

   - **Créer un Utilisateur (POST /api/v1/users/) :**
     ```python
     @api.route('/users/')
     class AdminUserCreate(Resource):
         @jwt_required()
         def post(self):
             current_user = get_jwt_identity()
             if not current_user.get('is_admin'):
                 return {'error': 'Admin privileges required'}, 403

             user_data = request.json
             email = user_data.get('email')

             if facade.get_user_by_email(email):
                 return {'error': 'Email already registered'}, 400

             pass
     ```

   - **Modifier un Utilisateur (PUT /api/v1/users/<user_id>) :**
     ```python
     @api.route('/users/<user_id>')
     class AdminUserModify(Resource):
         @jwt_required()
         def put(self, user_id):
             current_user = get_jwt_identity()
             if not current_user.get('is_admin'):
                 return {'error': 'Admin privileges required'}, 403

             data = request.json
             email = data.get('email')

             if email:
                 existing_user = facade.get_user_by_email(email)
                 if existing_user and existing_user.id != user_id:
                     return {'error': 'Email already in use'}, 400

             pass
     ```

   - **Ajouter une Commodité (POST /api/v1/amenities/) :**
     ```python
     @api.route('/amenities/')
     class AdminAmenityCreate(Resource):
         @jwt_required()
         def post(self):
             current_user = get_jwt_identity()
             if not current_user.get('is_admin'):
                 return {'error': 'Admin privileges required'}, 403

             pass
     ```

   - **Modifier une Commodité (PUT /api/v1/amenities/<amenity_id>) :**
     ```python
     @api.route('/amenities/<amenity_id>')
     class AdminAmenityModify(Resource):
         @jwt_required()
         def put(self, amenity_id):
             current_user = get_jwt_identity()
             if not current_user.get('is_admin'):
                 return {'error': 'Admin privileges required'}, 403

             pass
     ```

4. **Permettre aux Administrateurs de Contourner les Restrictions de Propriété**
   Lorsque l'administrateur tente de modifier ou supprimer un lieu ou un avis, les vérifications de propriété seront contournées.

   **Exemple pour Modifier un Lieu (PUT /api/v1/places/<place_id>) :**
   ```python
   @api.route('/places/<place_id>')
   class AdminPlaceModify(Resource):
       @jwt_required()
       def put(self, place_id):
           current_user = get_jwt_identity()

           is_admin = current_user.get('is_admin', False)
           user_id = current_user.get('id')

           place = facade.get_place(place_id)
           if not is_admin and place.owner_id != user_id:
               return {'error': 'Unauthorized action'}, 403

           pass
   ```

5. **Tester les Points de Terminaison Administrateur**

   - **Créer un Nouvel Utilisateur en tant qu'Administrateur :**
     ```bash
     curl -X POST "http://127.0.0.1:5000/api/v1/users/" -d '{"email": "newuser@example.com", "first_name": "Admin", "last_name": "User"}' -H "Authorization: Bearer <admin_token>" -H "Content-Type: application/json"
     ```

   - **Modifier les Données d'un Autre Utilisateur en tant qu'Administrateur :**
     ```bash
     curl -X PUT "http://127.0.0.1:5000/api/v1/users/<user_id>" -d '{"email": "updatedemail@example.com"}' -H "Authorization: Bearer <admin_token>" -H "Content-Type: application/json"
     ```

   - **Ajouter une Nouvelle Commodité en tant qu'Administrateur :**
     ```bash
     curl -X POST "http://127.0.0.1:5000/api/v1/amenities/" -d '{"name": "Swimming Pool"}' -H "Authorization: Bearer <admin_token>" -H "Content-Type: application/json"
     ```

   - **Modifier une Commodité en tant qu'Administrateur :**
     ```bash
     curl -X PUT "http://127.0.0.1:5000/api/v1/amenities/<amenity_id>" -d '{"name": "Updated Amenity"}' -H "Authorization: Bearer <admin_token>" -H "Content-Type: application/json"
     ```

---

Cette implémentation permettra aux administrateurs d'exécuter des opérations spécifiques sans les restrictions imposées aux utilisateurs standard, tout en assurant une sécurité via le contrôle d'accès basé sur les rôles.