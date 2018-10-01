# DRF DEMO PYYS 2018

> Simple restful API ready to be deployed on heroku.

## Tutorial

### Setting everything up

1. Create a virtualenv and install dependencies (we are gonna use `poetry` as package manager)

```bash
poetry add django djangorestframework markdown coreapi drf-yasg
```

```bash
poetry add flake8 mypy --dev
```

2. Create a django project

```bash
poetry run django-admin.py demo .
```

3. Add DRF to installed apps

```python
INSTALLED_APPS = (
    ...
    'rest_framework',
)
```

4. Add DRF settings

```python
REST_FRAMEWORK = {
    'DEFAULT_PERMISSION_CLASSES': (
        'rest_framework.permissions.AllowAny',
    ),
    'DEFAULT_RENDERER_CLASSES': [
        'rest_framework.renderers.JSONRenderer',
    ],
    'DEFAULT_PARSER_CLASSES': [
        'rest_framework.parsers.JSONParser',
    ]
}
```

5. Add the interactive documentation

Go to `demo.urls.py` and add this

```python
from rest_framework.documentation import include_docs_urls

urlpatterns = [
    ...
    url(r'^docs/', include_docs_urls(title='PYSS 2018'))
]
```

6. Add OpenAPI interactive docs (optional)

Install yasg `poetry add drf-yasg --extras validation`

Add to installed apps:

```python
INSTALLED_APPS = (
    ...
    'rest_framework',
    'drf_yasg',
)
```

In `demo.urls.py` we are gonna add:

```python
...
from drf_yasg.views import get_schema_view
from drf_yasg import openapi

...

schema_view = get_schema_view(
   openapi.Info(
      title="PYSS 2018",
      default_version='v1',
      description="Test description",
   ),
   validators=['flex', 'ssv'],
   public=True,
)

urlpatterns = [
   url(r'^openapi(?P<format>\.json|\.yaml)$', schema_view.without_ui(cache_timeout=0), name='schema-json'),
   url(r'^openapi/$', schema_view.with_ui('swagger', cache_timeout=0), name='schema-swagger-ui'),
   ...
]
```


### Working with DRF

We are gonna avoid creating models, so we are gonna just use `Users` from `django.contrib` as our model.

We are gonna supouse we are working with `customers`

1. Create a new app

```
poetry run python manage.py startapp customers
```

2. Add a `CustomerSerializer` to `customer.serializers.py` (create the file if it doesn't exist)
3. Add `CustomerViewSet` to `customer.views.py`
4. Add the `router` to `demo.urls.py`

**Done!** everything is set up

### Running the server

1. Run migrations `poetry run python manage.py migrate`
2. Run server `poetry run python manage.py runserver`


### Deploying to heroku

1. login to heroku.com
2. Go to `New` app