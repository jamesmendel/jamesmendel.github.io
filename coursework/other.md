---
# This page generates a list of courses that are NOT in the array below (departements)
# The list should be populated with all departments ON the sidebar so that they are
# ignored from this list. There's probably a more intelligent way to do this but meh.
layout: coursework
title: Misc Coursework
dept: Other
---
{% comment %}
    This array should contain all departments as seen
    in the sidebar in coursework.html
{% endcomment %}
{% assign departments = "MATH,EECS" | split:"," %}

# By Department - {{ page.dept | capitalize}}
-----

{% for course in site.data.coursework reversed %}
    {% unless departments contains course.dept %}

{% include course-list.md %}

    {% endunless %}
{% endfor %}