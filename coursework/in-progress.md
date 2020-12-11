---
# This page displays all classes which are in progress as marked by _config.yml
layout: coursework
title: In Progress Coursework
---

# By Category - In Progress
*{{ site.currentterm }}*

-----

{% for course in site.data.coursework reversed %}
    {% if course.term == site.currentterm %}

{% include course-list.md %}

    {% endif %}
{% endfor %}
