---
title: "Miscelanea"
date: 2021-10-21T10:36:54+02:00
weight: 45
description: >
  Configuraciones varias
tags: []
---

{{% pageinfo %}}

{{% /pageinfo %}}

## Jerarquías de páginas
* https://docs.wagtail.io/en/stable/topics/pages.html#page-type-business-rules
  
Definir ```parent_page_types```  y  ```subpage_types``` en modelos.

## Configuración editor
* https://docs.wagtail.io/en/stable/advanced_topics/customisation/page_editing_interface.html

```python
from wagtail.admin.edit_handlers import TabbedInterface, ObjectList

class BlogPage(Page):
    # field definitions omitted

    content_panels = [
        FieldPanel('title', classname="full title"),
        FieldPanel('date'),
        FieldPanel('body', classname="full"),
    ]
    sidebar_content_panels = [
        SnippetChooserPanel('advert'),
        InlinePanel('related_links', label="Related links"),
    ]

    edit_handler = TabbedInterface([
        ObjectList(content_panels, heading='Content'),
        ObjectList(sidebar_content_panels, heading='Sidebar content'),
        ObjectList(Page.promote_panels, heading='Promote'),
        ObjectList(Page.settings_panels, heading='Settings', classname="settings"),
    ])
```
## Limitar RichTextField
```body = RichTextField(features=['h2', 'h3', 'bold', 'italic', 'link'])```