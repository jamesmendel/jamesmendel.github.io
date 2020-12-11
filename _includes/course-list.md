## {{ course.dept }} {{ course.code }}
### {{ course.name }}
{{ course.desc }}
{% if course.link %}
[[More Info]({{ site.baseurl }}{{ course.link }})]
{% endif %}
{% if course.term == site.currentterm %}
***In Progress*** (*{{ course.term }}*)
{% else %}
(*{{ course.term }}*)
{% endif %}

-----
