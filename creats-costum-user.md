1. In <project>.settings
    1. in INSTALLED_APPS : 
        type the following in 
        '<app name>.apps.< name of class in apps>',

    2. At the bottom put the following :
        AUTH_USER_MODEL = '<app>.CustomUser'

2. In <app>.model :

    ```
    from django.contrib.auth.models import AbstractUser
    from django.db import models

    class CustomUser(AbstractUser):
        pass
        # add additional fields in here

        def __str__(self):
            return self.username
    ```

3. In <app> create file : forms.py

4. In <app>.form.py put the following :
    ```
    # accounts/forms.py
    from django import forms
    from django.contrib.auth.forms import UserCreationForm, UserChangeForm
    from .models import CustomUser

    class CustomUserCreationForm(UserCreationForm):

        class Meta:
            model = CustomUser
            fields = ('username', 'email')

    class CustomUserChangeForm(UserChangeForm):

        class Meta:
            model = CustomUser
            fields = ('username', 'email')
    ```

5. In <app>.admin.py put the following :

    ```
    from django.contrib import admin
    from django.contrib.auth import get_user_model
    from django.contrib.auth.admin import UserAdmin

    from .forms import CustomUserCreationForm, CustomUserChangeForm
    from .models import CustomUser

    class CustomUserAdmin(UserAdmin):
        add_form = CustomUserCreationForm
        form = CustomUserChangeForm
        model = CustomUser
        list_display = ['email', 'username',]

    admin.site.register(CustomUser, CustomUserAdmin)
    ```

6. And we're done! We can now run (in terminal) **makemigrations** and **migrate** for the first time to create a new database that uses the custom user model.

    ```
    (app) $ python manage.py makemigrations <app>
    (app) $ python manage.py migrate
    ```

7. In terminal, create super user : `python manage.py createsuperuser`

8. In settings, in TEMPLATES, in DIRS create link to your **templates** : 

    os.path.join(BASE_DIR,'tempaltes')
    * Dont forget to import os

9. In settings, at the bottom put :

    ```
    LOGIN_REDIRECT_URL = 'home'
    LOGOUT_REDIRECT_URL = 'home'
    ```

10. In the main folder root create folders **templates**  & **templates/registration**

11. Create four (4) templates files :

    templates/registration/**login.html**

    templates/registration/**signup.html**

    templates/**base.html**

    templates/**home.html**


12. update files with : 



    **base.html**

    ```
    <!-- templates/base.html -->
    <!DOCTYPE html>
    <html>
    <head>
    <meta charset="utf-8">
    <title>{% block title %}Django Auth Tutorial{% endblock %}</title>
    </head>
    <body>
    <main>
        {% block content %}
        {% endblock %}
    </main>
    </body>
    </html>
    ```

    **home.html**

    ```
    <!-- templates/home.html -->
    {% extends 'base.html' %}

    {% block title %}Home{% endblock %}

    {% block content %}
    {% if user.is_authenticated %}
    Hi {{ user.username }}!
    <p><a href="{% url 'logout' %}">Log Out</a></p>
    {% else %}
    <p>You are not logged in</p>
    <a href="{% url 'login' %}">Log In</a> |
    <a href="{% url 'signup' %}">Sign Up</a>
    {% endif %}
    {% endblock %}
    ```

    **login.html**

    ```
    <!-- templates/registration/login.html -->
    {% extends 'base.html' %}

    {% block title %}Log In{% endblock %}

    {% block content %}
    <h2>Log In</h2>
    <form method="post">
    {% csrf_token %}
    {{ form.as_p }}
    <button type="submit">Log In</button>
    </form>
    {% endblock %}
    ```

    **signup.html**

    ```
    <!-- templates/registration/signup.html -->
    {% extends 'base.html' %}

    {% block title %}Sign Up{% endblock %}

    {% block content %}
    <h2>Sign Up</h2>
    <form method="post">
    {% csrf_token %}
    {{ form.as_p }}
    <button type="submit">Sign Up</button>
    </form>
    {% endblock %}

    ```

13. In <project>.urls, fill the following :

    ```
    from django.contrib import admin
    from django.urls import path , include
    from django.views.generic.base import TemplateView

    urlpatterns = [
        path('', TemplateView.as_view(template_name='home.html'), name='home'),
        path('admin/', admin.site.urls),
        path('<app>/', include('<app>.urls')),
        path('<app>/', include('django.contrib.auth.urls')),
    ]

    ```
14. **Create** <app>.urls.py then **fill** it with :

    ```
    from django.urls import path
    from .views import SignUpView

    urlpatterns = [
        path('signup/', SignUpView.as_view(), name='signup'),
    ]
    ```

15. Last step, In <app>.views fill with :

    ```
    from django.urls import reverse_lazy
    from django.views.generic.edit import CreateView

    from .forms import CustomUserCreationForm

    class SignUpView(CreateView):
        form_class = CustomUserCreationForm
        success_url = reverse_lazy('login')
        template_name = 'registration/signup.html'
    ```

    * Run the server ☻