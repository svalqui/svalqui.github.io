# HTML

## Brakets


{% load python_module %}  -> Loads python_module.py
{% block my_block %}
{% include "dir/_scripts.html" %} -> includes other html files
{% endblock %}

{% block my_js %}
    <script src='dir/js/first.js' type='text/javascript' charset='utf-8'></script> -> executes script
{% endblock %}

{% extends 'base.html' %} -> this file extends the contents of base.html 

## Conditionals

### If
```
{% if field %}
    Do somthing
{% endif %}

{% if data|length == 0 %}
{% else %}
{% endif %}
```
### For
```
{% for error in field.errors %}
  <span class="help-block">{{ error }}</span>
{% endfor %}
```

