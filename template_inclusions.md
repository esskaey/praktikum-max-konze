# Inclusions
This format is used to include a html file (tags) directly in another file and pass arguments


## Simple Tag
A simple tag is used to perform logical operations on arguments or template context and return a display context


```python
# app/templatetags/model_tags.py
@register.simple_tag(takes_context=True)
def filtered_model_objects(context):
    if not context["filter_value"]:
        return Model.objects.without_content()
    else:
        return Model.objects.all()
```

Usage inside template
```html
{% extends "base.html" %}
{% block sidenav %}
{% filtered_model_objects as objects %}
{% endblock sidenav %}
```

## Inclusion Tag

### Include with arguments

```html
{% include "AppName/path/to_file.html" with argument=argument %}
```

### Include with pre-processing
```python
# app/templatetags/accordion.py
@register.inclusion_tag('content/containers/collapsible_accordion.html',takes_context=True)
def accordion_content(context,content=None,content_type='',name='Accordion Example',is_open=False,visible =True):
    return{
        "name":name,
        "context":context,
        "content":content,
        "content_type":content_type,
        "open":open,
        "visible":visible
    }

```

```html
{% comment %}'app_name/containers/collapsible_accordion.html'{% endcomment %}
{% load model_forms_templates %}
{% load model_filters %}
<!-- parameterizable accordion -->
<div class="accordion {% if not visible %} not-visible {% endif %}" id="{{ name|strip }}_id">
  <div class="accordion-item">
    <h2 class="accordion-header" id="{{ name|strip }}_heading">
      <button class="accordion-button" type="button" data-bs-toggle="collapse" data-bs-target="#{{ name|strip }}_target" aria-expanded="{{ open }}" aria-controls="{{ name|strip }}_control">
        {{ name }}
      </button>
    </h2>
    <div id="{{ name|strip }}_target" class="accordion-collapse collapse {% if open %}show{% endif %}" aria-labelledby="{{name|strip}}_heading">
        <div class="accordion-body" id="{{ name|strip }}_body">
        {% block accordion_content %}
          {% if content_type == "form" %}
            {% simple_form name=name form=content crispy=True %}
          {% elif content_type == "upload_form" %}
            {% include 'app_name/includes/upload_form.html' with url=url %}
          {% else %}
            {{ content }}
          {% endif %}
        {% endblock %}
      </div>
    </div>
</div>
```


## References

- [Django docs](https://docs.djangoproject.com/en/5.0/ref/templates/builtins/)