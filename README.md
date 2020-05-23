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

```python
import foobar

foobar.pluralize('word') # returns 'words'
foobar.pluralize('goose') # returns 'geese'
foobar.singularize('phenomena') # returns 'phenomenon'
```

## Contributing
Pull requests are welcome. For major changes, please open an issue first to discuss what you would like to change.

Please make sure to update tests as appropriate.
