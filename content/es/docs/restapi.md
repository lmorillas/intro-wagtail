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