---
layout: default
title: "A list of publications"
permalink: "/publications"
---

<ul>
  {% for publication in site.publications %}
    <li>
      <a href="{{ publication.url }}">{{ publication.title }}</a>
      - {{ publication.headline }}
    </li>
  {% endfor %}
</ul>
