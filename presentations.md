---
layout: default
title: "A list of presentations"
permalink: "/presentations"
---

<ul>
  {% for presentation in site.presentations %}
    <li>
      <a href="{{ presentation.url }}">{{ presentation.title }}</a>
      - {{ presentation.headline }}
    </li>
  {% endfor %}
</ul>
