# social-auth-app-django

social-auth-app-django is a Python library for creating a social authentication feature in your Django project.

## Installation

Use the package manager [pip](https://pip.pypa.io/en/stable/) to install foobar. It is expected that you have already installed django and python

```bash
pip install social-auth-app-django
pip install django-sslserver
```

I have used here a ssl localhost server because facebook requires a https server essentially so that django-sslserver pagage is used to force local host a https server.

## Usage
######social_app/setting.py
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

## Contributing
Pull requests are welcome. For major changes, please open an issue first to discuss what you would like to change.

Please make sure to update tests as appropriate.
