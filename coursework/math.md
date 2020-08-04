---
layout: coursework
title: Math Coursework
dept: MATH
---

# By Department - {{ page.dept | capitalize}}
-----

{% for course in site.data.coursework reversed %}
    {% if course.dept == page.dept %}

{% include course-list.md %}

    {% endif %}
{% endfor %}
