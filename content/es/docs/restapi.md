---
title: "REST API"
date: 2021-10-21T10:36:54+02:00
weight: 100
description: >
  REST API
tags: []
---

{{% pageinfo %}}
* https://docs.wagtail.org/en/stable/advanced_topics/api/index.html
* https://www.django-rest-framework.org/
{{% /pageinfo %}}

## Configuración 
https://docs.wagtail.org/en/stable/advanced_topics/api/v2/configuration.html

## Uso

### Obtener contenido
Puntos de acceso:
* Pages `/api/v2/pages/`
* Images `/api/v2/images/`
* Documents `/api/v2/documents/`

### Ejemplo
` $ curl  <sitio>/api/v2/pages/`

```json
{
    "meta": {
        "total_count": 34
    },
    "items": [
        {
            "id": 60,
            "meta": {
                "type": "base.HomePage",
                "detail_url": "http://127.0.0.1:8000/api/v2/pages/60/",
                "html_url": "http://127.0.0.1:8000/",
                "slug": "home",
                "first_published_at": "2019-02-10T16:24:31.388000Z",
                "locale": "en"
            },
            "title": "Welcome to the Wagtail Bakery!"
        },
        {
            "id": 3,
            "meta": {
                "type": "breads.BreadsIndexPage",
                "detail_url": "http://127.0.0.1:8000/api/v2/pages/3/",
                "html_url": "http://127.0.0.1:8000/breads/",
                "slug": "breads",
                "first_published_at": "2019-02-10T11:34:11.560000Z",
                "locale": "en"
            },
            "title": "Breads"
        },
        {
            "id": 34,
            "meta": {
                "type": "breads.BreadPage",
                "detail_url": "http://127.0.0.1:8000/api/v2/pages/34/",
                "html_url": "http://127.0.0.1:8000/breads/anadama-bread/",
                "slug": "anadama-bread",
                "first_published_at": "2019-02-10T13:00:21.882000Z",
                "locale": "en"
            },
            "title": "Anadama"
        },
``` 
### Campos personalizados
` $ curl  <sitio>/api/v2/pages/?fields=title,slug`

### Serializers
`$ curl <sitio>/api/v2/pages/?type=breads.BreadPage&fields=title,introduction,origin`

### Paginación
```
?limit=10
?offset=10
?limit=10&offset=10
```
### Ordenación

`?order=-title`
### Filtrado
`?slug=about`

### Búsqueda
`?search=James+Joyce&order=-first_published_at&search_operator=and`
<!--
## serializers
```python
from rest_framework.serializers import ModelSerializer
from .models import Pelicula

class PeliculaSerializer(ModelSerializer):
    class Meta:
        model = Pelicula
        fields = (
            'url', 'title', 'slug', 'rating', 'link', 'year', 'imagen', 'cast', 'generos'
        )
```

## views

```python
from rest_framework.mixins import (
    CreateModelMixin, ListModelMixin, RetrieveModelMixin, UpdateModelMixin
)
from rest_framework.viewsets import GenericViewSet

from rest_framework import permissions

from .models import Pelicula
from .serializers import PeliculaSerializer

'''
GenericViewSet  # generic view functionality
CreateModelMixin  # handles POSTs
RetrieveModelMixin  # handles GETs for 1 object
UpdateModelMixin,  # handles PUTs and PATCHes
ListModelMixin):  # handles GETs for many objects
'''


class PeliculaViewSet(ListModelMixin, GenericViewSet):  # handles GETs for many Companies
    ''' 
    API Endpoint para mostrar películas
    '''

    serializer_class = PeliculaSerializer
    queryset = Pelicula.objects.all().order_by('-rating')
    # permission_classes = [permissions.IsAuthenticated]
```

## urls
```python
# urls.py
from django.urls import include, path
from rest_framework.routers import DefaultRouter
from .views import PeliculaViewSet


router = DefaultRouter()
router.register(r'pelicula', PeliculaViewSet)

urlpatterns = [
    path('', include(router.urls)),
    path('api-auth/', include('rest_framework.urls', namespace='rest_framework'))
]
```

## installed apps 
```python
    'rest_framework',
```

-->
