# HTML

## Brakets

{% load python_module %}  -> Loads python_module.py


{% block my_block %}
{% include "dir/_scripts.html" %} -> includes other html files
{% endblock %}

{% block my_js %}
<script src='dir/js/first.js' type='text/javascript' charset='utf-8'></script> -> executes script
{% endblock %}
