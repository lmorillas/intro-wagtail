---
title: "Diagramas"
date: 2021-10-21T10:29:17+02:00
linkTitle: "Diagramas"
weight: 20
description: >
    
docs: >
    
    
---
{{% pageinfo %}}
## Documentación
* Generación de diagramas de forma eficiente
{{% /pageinfo %}}

## Graph_models
* con graph_models se pueden generar diagramas de modelos: https://django-extensions.readthedocs.io/en/latest/graph_models.html
> tienes que instalar django-extensions y los requisitos: `pygraphviz` o `pydot`

```bash
# Create a PNG image file called my_project_visualized.png with application grouping
$ python manage.py graph_models -a -g -o my_project_visualized.png
```

## Mermaid
* También podemos generar diagramas con mermaid: https://mermaid.js.org/
* ER diagram: https://mermaid-js.github.io/mermaid/#/entityRelationshipDiagram
* Gantt: https://mermaid.js.org/syntax/gantt.html
* Requisitos: https://mermaid.js.org/syntax/requirementDiagram.html
* Y muchos otros: https://mermaid.js.org/syntax/examples.html

Esos diagramas los podemos incluir y generar desde el markdown de la documentación: https://github.blog/2022-02-14-include-diagrams-markdown-files-mermaid/


