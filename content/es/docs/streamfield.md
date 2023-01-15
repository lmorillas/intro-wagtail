---
title: "Gestión de contenido mixto"
date: 2021-10-21T10:36:54+02:00
weight: 25
description: >
  Gestión de contenido mixto
tags: []
---

{{% pageinfo %}}
* https://docs.wagtail.io/en/stable/topics/streamfield.html
* https://www.accordbox.com/blog/how-use-streamfield-wagtail/
{{% /pageinfo %}}

```python
class BlogPage(Page):
    author = models.CharField(max_length=255)
    date = models.DateField("Post date")
    body = StreamField([
        ('heading', blocks.CharBlock(form_classname="full title")),
        ('paragraph', blocks.RichTextBlock()),
        ('image', ImageChooserBlock()),
    ])

    content_panels = Page.content_panels + [
        FieldPanel('author'),
        FieldPanel('date'),
        StreamFieldPanel('body'),
    ]
```

```html
<article>
    {% for block in page.body %}
        {% if block.block_type == 'heading' %}
            <h1>{{ block.value }}</h1>
        {% else %}
            <section class="block-{{ block.block_type }}">
                {% include_block block %}
            </section>
        {% endif %}
    {% endfor %}
</article>
```
```python
 body = StreamField([
     ('person', blocks.StructBlock([
         ('first_name', blocks.CharBlock()),
         ('surname', blocks.CharBlock()),
         ('photo', ImageChooserBlock(required=False)),
         ('biography', blocks.RichTextBlock()),
     ])),
     ('heading', blocks.CharBlock(form_classname="full title")),
     ('paragraph', blocks.RichTextBlock()),
     ('image', ImageChooserBlock()),
 ])
 ```

 ```html
 <article>
    {% for block in page.body %}
        {% if block.block_type == 'person' %}
            <div class="person">
                {% image block.value.photo width-400 %}
                <h2>{{ block.value.first_name }} {{ block.value.surname }}</h2>
                {{ block.value.biography }}
            </div>
        {% else %}
            (rendering for other block types)
        {% endif %}
    {% endfor %}
</article>
```