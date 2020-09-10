---
layout: coursework
title: Coursework
---

# Coursework
### A comprehensive list of courses I have completed at KU.
-----

{% for course in site.data.coursework reversed %}

{% include course-list.md %}

{% endfor %}


