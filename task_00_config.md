### Tâche : Modifier l'Application Factory pour inclure la Configuration

#### Objectif
Mettre à jour l'Application Factory Flask pour inclure l'objet de configuration.

#### Contexte
Dans la partie précédente du projet, nous avons créé une classe `Config` pour gérer différentes configurations dans l'application, mais nous ne l'utilisions pas encore. Dans cette tâche, vous allez mettre à jour la méthode `create_app()` (suivant le modèle Application Factory) dans le fichier `app/__init__.py` pour recevoir une configuration, qui sera utilisée pour instancier l'application.

> [!IMPORTANT]
> Avant de commencer la tâche, assurez-vous de lire et de comprendre les ressources fournies ci-dessous.

#### Instructions

1. **Mettre à jour la méthode `create_app()` pour recevoir une configuration**
   - Dans le fichier `app/__init__.py` :
     - Mettez à jour la méthode `create_app()` pour recevoir un objet de configuration.
     - Modifiez la méthode pour instancier l'application Flask en utilisant l'objet de configuration.
     - Retournez l'instance d'application créée.

   ```python
   def create_app(config_class="config.DevelopmentConfig"):
       app = Flask(__name__)
       app.config.from_object(config_class)
       # ...
   ```

   Nous définirons la classe `config.DevelopmentConfig` comme valeur par défaut.

#### Ressources
- **Documentation Flask :** [Application Factories](https://flask.palletsprojects.com/en/stable/patterns/appfactories/)
- **Documentation Flask :** [Configuration Handling](https://flask.palletsprojects.com/en/stable/config/)

#### Résultat attendu
À la fin de cette tâche, vous devriez avoir une Application Factory entièrement fonctionnelle capable de gérer différentes configurations.

Citations:
[1] https://towardsdatascience.com/how-to-set-up-a-production-grade-flask-application-using-application-factory-pattern-and-celery-90281349fb7a
[2] https://flask-appfactory.readthedocs.io/en/latest/tut_app.html
[3] https://flask.palletsprojects.com/en/stable/tutorial/factory/
[4] https://flask.palletsprojects.com/en/stable/patterns/appfactories/
[5] https://flask.palletsprojects.com/en/3.0.x/config/
[6] https://hackersandslackers.com/flask-application-factory/
[7] https://bobwaycott.com/blog/how-i-use-flask/flask-app-factory-pattern/
[8] https://dev.to/bredmond1019/flask-application-factory-1j81
