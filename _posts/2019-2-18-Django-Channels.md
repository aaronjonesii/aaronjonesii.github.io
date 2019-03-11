---
layout: post
title: Django Channels
image: https://user-images.githubusercontent.com/18272237/52909263-0d1a9b80-3254-11e9-9a97-3c818005ff0f.png
---
Django REST-API >> Channels >> Redis >> MySQL

## Create routing configuration file
```python
# mysite/routing.py
from channels.routing import ProtocolTypeRouter

application = ProtocolTypeRouter({
    # (http->django views is added by default)
})
```

## Add Channels library to `INSTALLED_APPS`
```python
# mysite/settings.py
INSTALLED_APPS = [
    'channels',
    '<APP-NAME>',
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
]
```

### Point Channels to the routing configuration
Add this to the bottom of `mysite/settings.py`
```python
# mysite/settings.py
# Channels
ASGI_APPLICATION = 'mysite.routing.application'
```

## Test Server
```bash
python3 manage.py runserver
```
You should see `Starting ASGI/Channels development server at http://127.0.0.1:8000/`
indicating that the Channels developemtn server has taken over




***
[Django Channel Tutorial Reference](https://channels.readthedocs.io/en/latest/tutorial/part_1.html)

[Django Channels Deploy Reference](https://channels.readthedocs.io/en/latest/deploying.html)
