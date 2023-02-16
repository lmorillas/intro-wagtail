---
title: "CORS"
date: 2021-10-21T10:29:17+02:00
linkTitle: "CORS"
weight: 400
description: >
    
---
{{% pageinfo %}}
## Documentación
* https://github.com/adamchainz/django-cors-headers

{{% /pageinfo %}}

## Install from pip
```bash
python -m pip install django-cors-headers
```

and then add it to your installed apps:

```python
INSTALLED_APPS = [
    ...,
    "corsheaders",
    ...,
]

MIDDLEWARE = [
    ...,
    "corsheaders.middleware.CorsMiddleware",
    "django.middleware.common.CommonMiddleware",
    ...,
]

```

## Configuración
```python
CORS_ALLOWED_ORIGINS
CORS_ALLOWED_ORIGIN_REGEXES
CORS_ALLOW_ALL_ORIGINS
```


