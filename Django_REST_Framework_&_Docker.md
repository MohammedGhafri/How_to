1. Prepare your environment for the project
    - > mkdir
    - > cd 
    - > poetry add django django djangorestframework
    - > You can do migration
    - > 

2. project.settings :

    - > Add to INSTALLED_APPS :
    'rest_framework',
    'app.apps.AppConfig',

    - > At the bottom : 
        ```
        REST_FRAMEWORK = {
            'DEFAULT_PERMISSION_CLASS': [
            'rest_framework.permissions.AllowAny',
            ]
        }
        ``` 

3. app.model :

    - > Create class for your desired information i.e : auther , body , ....etc.
    - > dont forget to import the desired moduls

4. As you created a model, do the migration by 

    - > python manage.py makemigrations <name of app>
    - > python manage.py migrate

5. Copy and paste the tests into **your** app.tests the tests as its from demo.

6. In project.urls :

    - > add to urlpatterns path to app.urls
    - > Dont forget to import **include**

7. In app.urls :
    - > create urlpatterns 
    - > add to urlpatterns the desired pathes and dont forget to import the desired.

8. In app folder :

    - > Create file `serializer.py`.
    - > Add the desired view of your model
    - > dont forget to import the desired

9. In app.views :
    
    - > Create classes the related to serializer as explained in the demo

10. Before run server, create superuser 



