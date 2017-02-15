---
layout: archive
author_profile: true
permalink: /work/
---

<div class="grid__wrapper">
  {% for post in site.work %}
    {% include archive-single.html type="grid" %}
  {% endfor %}
</div>
