---
title: "Cómo usar imágenes"
date: 2021-10-21T10:36:54+02:00
weight: 20
description: >
  Usando imágenes en plantillas
tags: []
---

{{% pageinfo %}}
https://docs.wagtail.io/en/stable/topics/images.html
{{% /pageinfo %}}

```
{% image [image] [resize-rule] %}
```

## Resize rules
* `width`: Resize to width
* `height`: Resize to height
* `width,height`: Resize to width and height
* `width,height,crop`: Resize to width and height and crop
* `fill`: Resize to width and height and crop to fill
* `max-widthxheight`: Resize to max
* `scale`: Scale to width and height
* `original`: Use original image

