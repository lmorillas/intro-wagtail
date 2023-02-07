---
title: "Docker"
date: 2021-10-21T10:29:17+02:00
linkTitle: "Docker"
weight: 500
description: >
  Despliegue con contenedores
---
{{% pageinfo %}}
## Documentación
* https://josedom24.github.io/curso_docker_2022/sesion1/
* https://plataforma.josedomingo.org/pledin/cursos/openshift/curso/u03/index.html
* https://sites.google.com/site/desplieguedeaplicacionesweb/tema-6-contenedores

### Nginx
* https://github.com/twtrubiks/docker-django-nginx-uwsgi-postgres-tutorial/blob/master/README.en.md
* https://blog.devgenius.io/how-to-dockerize-a-production-ready-django-application-django-nginx-uwsgi-a908d3e4d8f8
* https://testdriven.io/blog/dockerizing-django-with-postgres-gunicorn-and-nginx/

{{% /pageinfo %}}

> Aplicación a nuestro entorno de desarrollo web
> Nosotros desplegamos los contenedores dentro de un servidor virtualizado con `Vagrant`.

## Ejemplo 1
* `docker-compose.yml` con servicios de `postgresql` y `redis`
* `wagtail` accede a `postgresql` y a `redis` por los puertos que mapeamos.

```yaml
version: '3.3'  
services:
  db:
    image: postgres:latest
    restart: always
    environment:
      - POSTGRES_USER
      - POSTGRES_PASSWORD
      - POSTGRES_DB
    ports:
      - '5432:5432'
    volumes: 
      - dbbakery:/var/lib/postgresql/data
      - ./backup:/backup
      # - ./inicio/backup.dump.sql:/docker-entrypoint-initdb.d/create_tables.sql

  adminer:
    image: adminer
    restart: always
    ports:
      - 80:8080
    environment:
      - ADMINER_DEFAULT_SERVER=db

  redis:
    image: redis:latest
    restart: always
    ports:
      - '6379:6379'

volumes:
  dbbakery:
    driver: local

```

## Ejemplo 2
* `docker-compose.yml` con servicio de `wagtail`

```yaml
version: '3.3'
services:
  db:
    environment:
      - POSTGRES_USER
      - POSTGRES_PASSWORD
      - POSTGRES_DB
    restart: unless-stopped
    image: postgres
    expose:
      - '5432'
    volumes:
      - dbbakery:/var/lib/postgresql/data
      - ./backup:/backup

  redis:
    restart: unless-stopped
    image: redis
    expose:
      - '6379'
  app:
    environment:
      DJANGO_SECRET_KEY: changeme
      DATABASE_URL: postgres://alumno:changeme@db/bakerypg
      CACHE_URL: redis://redis
    build:
      context: .
      dockerfile: ./Dockerfile
    volumes:
      - media-root:/code/bakerydemo/media
    links:
      - db:db
      - redis:redis
    ports:
      - '8000:8000'
    depends_on:
      - db
      - redis
volumes:
  media-root:
  dbbakery:

```