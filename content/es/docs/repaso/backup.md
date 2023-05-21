---
title: "Copias de seguridad"
date: 2021-10-21T10:29:17+02:00
linkTitle: "Backup"
weight: 50
description: >
    
doc: >

---
{{% pageinfo %}}
## Objetivo:
* Hacer copias de seguridad del sitio para poder compartir la información en otro servidor
* Migrar de SQLite a PostgreSQL o MySQL
* https://www.yellowduck.be/posts/migrating-your-wagtail-site-to-a-different-database-engine/
{{% /pageinfo %}}

## Dumpdata
### Creación del volcado de datos

```bash
python manage.py dumpdata --natural-foreign --indent 2 \
    -e contenttypes -e auth.permission \
    -e wagtailcore.groupcollectionpermission \
    -e wagtailcore.grouppagepermission -e wagtailimages.rendition \
    -e sessions > data.json
```

> Reemplaza `\` por `^` en Windows

### Preparación de la base de datos: elimina páginas existentes
```bash
python manage.py shell
>>> from wagtail.core.models import Page
>>> Page.objects.all().delete()
>>> exit()
```

### Carga de datos
```bash
python manage.py loaddata data.json
```

> Si algo no funciona, revisa este enlace: https://github.com/wagtail/bakerydemo/#preparing-this-archive-for-distribution