# social-auth-app-django

social-auth-app-django is a Python library for creating a social authentication feature in your Django project.

## Installation

Use the package manager [pip](https://pip.pypa.io/en/stable/) to install foobar. It is expected that you have already installed django and python

```bash
pip install social-auth-app-django
pip install django-sslserver
```

*I have used here a ssl localhost server because facebook requires a https server essentially so that django-sslserver pagage is used to force local host a https server.

## Usage

```bash
django-admin startproject social_app
django manage.py startapp core
cd core
mkdir templates
mkdir static
```

###### social_app/setting.py
```python
#python socio auth
    'social_django', # add this
    'core', # add this

    #adding ssl server app to force localhost https
    'sslserver',
    
MIDDLEWARE = [
    .
    .
    .

    #custom social auth setting
    'social_django.middleware.SocialAuthExceptionMiddleware',
]



TEMPLATES = [
    {
        .
        .
        .
        
        'OPTIONS': {
            'context_processors': [
                .
                .
                .
                

                #custom social auth setting
                'social_django.context_processors.backends',
            ],
        },
    },
]

#social_app custom settings

AUTHENTICATION_BACKENDS = [
    'social_core.backends.google.GoogleOAuth2',
    'social_core.backends.facebook.FacebookOAuth2',
    'django.contrib.auth.backends.ModelBackend',
]


SOCIAL_AUTH_PIPELINE = (
    'social_core.pipeline.social_auth.social_details',
    'social_core.pipeline.social_auth.social_uid',
    'social_core.pipeline.social_auth.social_user',
    'social_core.pipeline.user.get_username',
    'social_core.pipeline.social_auth.associate_by_email',
    'social_core.pipeline.user.create_user',
    'social_core.pipeline.social_auth.associate_user',
    'social_core.pipeline.social_auth.load_extra_data',
    'social_core.pipeline.user.user_details',
)

LOGIN_URL = 'login'
LOGIN_REDIRECT_URL = 'home'
LOGOUT_URL = 'logout'
LOGOUT_REDIRECT_URL = 'login'


SOCIAL_AUTH_FACEBOOK_KEY = "your_facebook_appid"        # App ID
SOCIAL_AUTH_FACEBOOK_SECRET = "your_facebook_secretkey"  # App Secret
SOCIAL_AUTH_GOOGLE_OAUTH2_KEY = "your_google_clientId"        # App ID
SOCIAL_AUTH_GOOGLE_OAUTH2_SECRET = "your_google_clientsecret"  # App Secret

SOCIAL_AUTH_FACEBOOK_EXTRA_DATA = [                 # add this
    ('name', 'name'),
    ('email', 'email'),
    ('picture', 'picture'),
    ('link', 'profile_url'),
]
```
###### social_app/urls.py
```python
# social_app/urls.py

from django.contrib import admin
from django.urls import path, include
from django.contrib.auth import views as auth_views
from core import views

urlpatterns = [
    path('admin/', admin.site.urls),


    #Adding social auth path
    path('social-auth/', include('social_django.urls', namespace="social")),
    
    path("", views.home, name="home"),
    path("login/", views.login, name="login"),
    path("logout/", auth_views.LogoutView.as_view(), name="logout"),
]
```
###### social_app/views.py
```python
from django.shortcuts import render

# Create your views here.
# core/views.py

from django.shortcuts import render
from django.contrib.auth.decorators import login_required

# Create your views here.
def login(request):
  return render(request, 'login.html')

@login_required
def home(request):
  return render(request, 'home.html')
```
###### core/templates/base.html
```html
<!-- templates/base.html -->

{% load static %}
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <meta http-equiv="X-UA-Compatible" content="ie=edge" />
    <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.1.3/css/bootstrap.min.css" integrity="sha384-MCw98/SFnGE8fJT3GXwEOngsV7Zt27NXFoaoApmYm81iuXoPkFOJwJ8ERdknLPMO"
        crossorigin="anonymous" />
    <link rel="stylesheet" href="{% static 'css/index.css' %}" />
    <title>Social Auth with Django</title>
</head>
<body>
    <div class="container-fluid">
        <div>
            <h1 class="text-white text-center">{% block title %}{% endblock %}</h1>
            <div class="card p-5">
                {% block content %}
                {% endblock %}
            </div>
        </div>
    </div>
</body>
</html>
```
###### core/templates/home.html
```html
<!-- templates/home.html -->

{% extends 'base.html' %}
{% block title %} Home {% endblock %}
{% block content %}
<div class="row">
    <div class="col-sm-12 mb-3">
        <h4 class="text-center"> Welcome {{ user.username }} </h4>
    </div>
</div>
{% endblock %}
```

###### core/templates/login.html
```html
<!-- templates/login.html -->

{% extends 'base.html' %}
{% block title %} Sign in {% endblock %}
{% block content %}
<div class="row">
    <div class="col-md-8 mx-auto social-container my-2 order-md-1">
        <button class="btn btn-danger  mb-2">
            <a href="{% url 'social:begin' 'google-oauth2' %}">Login with google</a>
        </button>
        <button class="btn btn-primary mb-2">
            <a href="{% url 'social:begin' 'facebook' %}">Login with Facebook</a>
        </button>
        <button class="btn btn-info mb-2">
            <a href="#">Login with LinkedIn</a>
        </button>
    </div>
</div>
</div>
{% endblock %}
```
###### static/index.css
```css
img {
  border: 3px solid #282c34;
}
.container-fluid {
  height: 100vh;
  background-color: #282c34;
  display: flex;
  justify-content: center;
  align-items: center;
}
.container-fluid > div {
  width: 85%;
  min-width: 300px;
  max-width: 500px;
}
.card {
  width: 100%;
}
.social-container {
  display: flex;
  flex-direction: column;
  justify-content: center;
}
.btn a, .btn a:hover {
  color: white;
  text-decoration: none ;
}
```

```bash
python manage.py migrate
```
## Contributing
Pull requests are welcome. For major changes, please open an issue first to discuss what you would like to change.

Please make sure to update tests as appropriate.
