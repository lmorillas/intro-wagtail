---
title: "Instalación"
date: 2021-10-21T10:36:54+02:00
weight: 5
description: >
  Instalación entorno
tags: [wagtail, instalación]
---

{{% pageinfo %}}
Información general de Wagtail:
* https://wagtail.io/

Instalación: 
* https://docs.wagtail.io/en/stable/getting_started/index.html#quick-install
{{% /pageinfo %}}

## Instalación con un entorno virtual
```bash
# preparación directorio
$ mkdir wagtail_project && cd wagtail_project
# creación y activación del entorno
$ python3 -m venv env
$ source env/bin/activate
# instalar wagtail
(env)$ pip install wagtail
# creación de un sitio con wagtail
(env)$ wagtail start wagtail_bootstrap_blog .
# crear tablas bd
(env)$ python manage.py migrate
(env)$ python manage.py runserver
```
El sitio estará funcionando en http://127.0.0.1:8000/ 

Archivos creados:
```
Dockerfile
db.sqlite3
env
home
manage.py
requirements.txt
search
wagtail_bootstrap_blog
```

## Fíjate:
1. `Dockerfile` es para despliegue con docker
2. `db.sqlite3` es la bbdd local
3. `home` y `search` son aplicaciones django que crea wagtail por defecto