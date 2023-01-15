---
title: "Aplicación películas"
date: 2021-10-21T10:36:54+02:00
weight: 85
description: >
  Aplicación de películas
tags: []
---

{{% pageinfo %}}
Crear una aplicación de películas con la informaciń del siguiente archivo json. Es la lista de las películas más vistas de [imdb.com](https://www.imdb.com/chart/top/?ref_=nv_mv_250_6).
{{% /pageinfo %}}


* [Películas](listapelis.json)

## Ayudas

### Ejecutar script de django
```bash
python manage.py shell < url_script.py
```

### slugify
* https://docs.djangoproject.com/en/4.0/ref/utils/#django.utils.text.slugify
* para rellenar campos **models.SlugField()**
* Ejemplo: https://learndjango.com/tutorials/django-slug-tutorial



### Paginación
#### Modelo
**Ejemplo breads:** https://github.com/wagtail/bakerydemo/blob/9394d3a779d1cc5c95ca08281b722c6b2dc3f58d/bakerydemo/breads/models.py
```python
class XXXX(Page):
    ....
    # Returns a queryset of BreadPage objects that are live, that are direct
    # descendants of this index page with most recent first
    def get_breads(self):
        return BreadPage.objects.live().descendant_of(
            self).order_by('-first_published_at')

    # Allows child objects (e.g. BreadPage objects) to be accessible via the
    # template. We use this on the HomePage to display child items of featured
    # content
    def children(self):
        return self.get_children().specific().live()

    # Pagination for the index page. We use the `django.core.paginator` as any
    # standard Django app would, but the difference here being we have it as a
    # method on the model rather than within a view function
    def paginate(self, request, *args):
        page = request.GET.get('page')
        paginator = Paginator(self.get_breads(), 12)
        try:
            pages = paginator.page(page)
        except PageNotAnInteger:
            pages = paginator.page(1)
        except EmptyPage:
            pages = paginator.page(paginator.num_pages)
        return pages

    # Returns the above to the get_context method that is used to populate the
    # template
    def get_context(self, request):
        context = super(BreadsIndexPage, self).get_context(request)

        # BreadPage objects (get_breads) are passed through pagination
        breads = self.paginate(request, self.get_breads())

        context['breads'] = breads

        return context
```

#### Plantilla
* https://github.com/wagtail/bakerydemo/blob/63c4add18afe07949efb3a7166a423cdef1e8a47/bakerydemo/templates/breads/breads_index_page.html
* https://github.com/wagtail/bakerydemo/blob/63c4add18afe07949efb3a7166a423cdef1e8a47/bakerydemo/templates/includes/pagination.html
```html
 {% if breads.paginator.count > 1 %}
        <div class="container">
            <div class="row">
                <div class="col-md-12">
                {% include "includes/pagination.html" with subpages=breads %}
                </div>
            </div>
        </div>
 {% endif %}

  
<nav role="pagination" aria-label="Pagination">
    <ul class="pagination">
        {% if subpages.has_previous %}
            <li class="page-item">
                <a href="?page={{ subpages.previous_page_number }}" class="page-link previous arrows">previous</a>
            </li>
            {% else %}
            <li class="page-item disabled">
                <a class="page-link">previous</a>
            </li>
        {% endif %}

        {% for i in subpages.paginator.page_range %}
            {% if subpages.number == i %}
              <li class="active"><span>{{ i }} <span class="sr-only">(current)</span></span></li>
            {% else %}
              <li class="page-item"><a href="?page={{ query_string|urlencode }}&amp;page={{ i }}" class="page-link">{{ i }}</a></li>
            {% endif %}
          {% endfor %}

        {% if subpages.has_next %}
            <li class="page-item">
                <a href="?page={{ subpages.next_page_number }}" class="page-link next arrows">next</a>
            </li>
            {% else %}
            <li class="page-item disabled">
                <a class="page-link">next</a>
            </li>
        {% endif %}
    </ul>
</nav>


```