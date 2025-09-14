---
layout: page
title: "Projects"
permalink: /projects/
---
A concise overview of selected personal and team projects.

<ul>
{% assign items = site.projects | sort: "title" %}
{% for p in items %}
  <li><a href="{{ p.url | relative_url }}">{{ p.title }}</a> — {{ p.description }}</li>
{% endfor %}
</ul>
