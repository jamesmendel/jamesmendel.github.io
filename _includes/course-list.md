## {{ course.dept }} {{ course.code }}
### {{ course.name }}
{% if course.term == site.currentterm %}
***In Progress*** - {{ course.desc }}
{% else %}
*{{ course.term }}* - {{ course.desc }} 
{% endif %}
{% if course.link %}
[[More Info]({{ site.baseurl }}{{ course.link }})]
{% endif %}

-----
