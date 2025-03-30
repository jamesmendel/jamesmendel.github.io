---
# Main projects page that lists all posts with category = project
layout: projects
title: Projects
---

----

{% for project in site.categories.project %}

## {{ project.title }} <small>(*{{ project.date | date: "%Y" }}*)</small>
*{{ project.description }}*

<!-- {{ project.excerpt }} -->
{{ project.content | markdownify | strip_html | truncatewords: 50 }}

[[Read More]({{ site.baseurl }}{{ project.url | remove: ".html" }})]

----

{% endfor %}
