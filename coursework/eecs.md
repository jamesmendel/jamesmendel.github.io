---
layout: coursework
title: EECS Coursework
dept: EECS
---

# By Department - {{ page.dept }}
-----

{% for course in site.data.coursework reversed %}
    {% if course.dept == page.dept %}

{% include course-list.md %}

    {% endif %}
{% endfor %}
