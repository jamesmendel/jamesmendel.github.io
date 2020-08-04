---
layout: frontpage
title: Projects
---

# Projects

{% for project in site.data.projects %}

## {{ project.name }}

---

{{ project.description }}

[[More Info]({{ project.url }})]



{% endfor %}
