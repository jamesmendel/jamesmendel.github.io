---
# This page displays all classes with the hnrs tag marked true.
layout: coursework
title: Honors Coursework
---

# By Category - Honors
-----

{% for course in site.data.coursework reversed %}
    {% if course.hnrs %}

{% include course-list.md %}

    {% endif %}
{% endfor %}
