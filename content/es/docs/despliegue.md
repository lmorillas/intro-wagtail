---
title: "Despliegue"
date: 2021-10-21T10:36:54+02:00
weight: 90
description: >
  Servidor en producción
tags: []
docs: >
  * https://www.endpointdev.com/blog/2022/06/how-to-deploy-django-app-with-aurora-serverless-and-copilot/
  * https://www.endpointdev.com/blog/2022/06/how-to-deploy-containerized-django-app-with-aws-copilot/
  * https://www.digitalocean.com/community/tutorials/how-to-set-up-django-with-postgres-nginx-and-gunicorn-on-ubuntu-18-04
  * https://arctype.com/blog/install-django-ubuntu/
  * https://www.linkedin.com/pulse/how-deploy-django-application-aws-ubuntu-ec2-nginx-uwsgi-yiqing-lan/


  * https://github.com/testdrivenio/django-docker-s3/blob/master/docker-compose.yml
  * https://testdriven.io/blog/storing-django-static-and-media-files-on-amazon-s3/
  * https://realpython.com/django-nginx-gunicorn/#making-your-site-production-ready-with-https
  * https://web.archive.org/web/20220108092237/https://engineertodeveloper.com/category/wagtail/
  * https://github.com/neon-jungle/wagtail-birdsong
  * https://blog.ichasco.com/nginx-mejorar-el-rendimiento-con-la-cache/

  * https://github.com/cfpb/consumerfinance.gov

---

{{% pageinfo %}}
 * https://docs.wagtail.org/en/stable/advanced_topics/deploying.html
 * https://docs.djangoproject.com/en/stable/howto/deployment/
 * https://docs.wagtail.org/en/stable/advanced_topics/performance.html

{{% /pageinfo %}}

![Despliegue](https://files.realpython.com/media/nginx-gunicorn-final-config.f31311dc4fed.png)

## CHECKLIST

https://docs.djangoproject.com/en/4.0/howto/deployment/checklist/


## Requisitos
* Postgresql
* Elasticsearch
* Redis
* Varnish ? 
* Gunicorn



### BASE
```bash
sudo apt update
sudo apt install python3-pip python3-dev libpq-dev postgresql postgresql-contrib nginx redis-server
```

### Entorno
* Sube los archivos del proyecto (sin el entorno virutal) o comparte la carpeta con el servidor
* Genera de nuevo el entorno virtual
* Instala ```gunicorn```

```bash
gunicorn <miproyecto>.wsgi:application --bind 0.0.0.0:8000 --chdir=/<ruta_base>
```


#### Gunicorn 
* https://docs.gunicorn.org/en/latest/deploy.html
  
### Nginx 

```nginx
upstream app_django {
    server web:8000;
}

server {
    listen 80;
    location / {
        proxy_pass http://app_django;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header Host $host;
        proxy_redirect off;
        client_max_body_size 20M;
    }
    location /static/ {
        alias /app/static/;
    }
    location /media/ {
        alias /app/media/;
    }
}
```

### Postgresql

* Crear base de datos
* Crear usuario y dar permisos
  
```bash
sudo -u postgres psql

CREATE DATABASE myproject;
CREATE USER myprojectuser WITH PASSWORD 'password';
GRANT ALL PRIVILEGES ON DATABASE myproject TO myprojectuser;
\q
```

#### Configuración en settings
{{% pageinfo %}}
Recuerda el uso de ```dotenv```
* https://github.com/jpadilla/django-dotenv
* https://github.com/theskumar/python-dotenv
{{% /pageinfo %}}
```python
import os
DATABASES = {
  "default": {
    "ENGINE": os.environ.get("SQL_ENGINE", "django.db.backends.sqlite3"),
    "NAME": os.environ.get("SQL_DATABASE", os.path.join(BASE_DIR, "db.sqlite3")),
    "USER": os.environ.get("SQL_USER", "user"),
    "PASSWORD": os.environ.get("SQL_PASSWORD", "password"),
    "HOST": os.environ.get("SQL_HOST", "localhost"),
    "PORT": os.environ.get("SQL_PORT", "5432"),
  }
}
```

Modificar ```wsgi.py``` o configurar en ```.env``` ```DJANGO_SETTINGS_MODULE```
```python
import os
from django.core.wsgi import get_wsgi_application
os.environ.setdefault("DJANGO_SETTINGS_MODULE", "<miproyecto>.settings.production")
application = get_wsgi_application()
```

