---
description: Add a page to list works.
---

# Work listing page

## In this section

* Adding a view and template to list all works
* How to link to an expression
* Getting the latest expression for a work

## Work listing view

On our app's homepage, we're going to list all the **works**.

Let's create the View code to handle this in `views.py`. We'll use Django's generic `ListView` view which does all the work for us.

{% code title="views.py" %}
```python
from django.views.generic import ListView
from .models import Work

class WorkListView(ListView):
    model = Work
```
{% endcode %}

Now create the matching template. Django will automatically look for a file called `templates/reader/work_list.html`:

{% code title="work_list.html" %}
```html
<html>
<head><title>All my works</title></head>
<body>
  <h1>Cape Town By-laws</h1>
  
  <table>
    {% raw %}
{% for work in work_list %}
      <tr>
        <td>{{ work.title }}</td>
        <td>{{ work.frbr_uri }}</td>
      </tr>
    {% endfor %}
{% endraw %}
  </table>
</body>
```
{% endcode %}

Finally, let's add this view `legislation/urls.py`:

{% code title="urls.py" %}
```python
from django.urls import path
from reader.views import *

urlpatterns = [
    path('', WorkListView.as_view(), name="home"),
]
```
{% endcode %}

Now we need to run our server:

```
python manage.py runserver
```

Visit [http://localhost:8000](http://localhost:8000) and see you list of by-laws!

We now have a basic listing page that shows the titles and FRBR URIs of all the works.

How can we create a link to read the content of a by-law? If we change the table to make the title a link, what exactly are we linking to?

## Linking to an expression

Recall that a work may have multiple expressions. For example, there could be different languages and different amended versions of a work.

When we link to the content of a work, which expression are we linking to?

For our app, we want to link to the **latest version** of a work. We're also going to add a `DEFAULT_LANGUAGE_CODE` setting for the site. We'll look for expressions that are in that language first, and then fall back to any language.

Let's add a helper method `latest_expression` to the Work model to help us fetch the latest expression for the work.

{% code title="models.py" %}
```python
from django.db import models
from django.conf import settings

default_language_code = settings.READER_APP_SETTINGS['DEFAULT_LANGUAGE_CODE']


class Work(models.Model):
    frbr_uri = models.CharField(max_length=512, unique=True)
    title = models.CharField(max_length=512)
    metadata = models.JSONField()

    def __str__(self):
        return f'{self.title} ({self.frbr_uri})'

    def default_expression(self):
        # first, try to get the latest expression in the default language
        expression = self.expressions.filter(language_code=default_language_code).first()
        # otherwise, get the latest expression in any other language
        if not expression:
            expression = self.expressions.first()

        return expression
```
{% endcode %}

Let's add the new `DEFAULT_LANGUAGE_CODE` to `legislation/settings.py`:

{% code title="settings.py" %}
```python
# ...

READER_APP_SETTINGS = {
    'DEFAULT_LANGUAGE_CODE': 'eng',
}
```
{% endcode %}

Now we can update the template to link to the latest expression.

{% code title="work_list.html" %}
```html
<html>
<head><title>All my works</title></head>
<body>
  <h1>Cape Town By-laws</h1>
  
  <table>
    {% raw %}
{% for work in work_list %}
      <tr>
        <td>
          {% with work.default_expression as expr %}
            {% if expr %}
              <a href="{% url 'expression' expr.frbr_uri %}">{{ expr.title }}</a>
            {% else %}
              {{ work.title }}
            {% endif %}
          {% endwith %}
        </td>
        <td>{{ work.frbr_uri }}</td>
      </tr>
    {% endfor %}
{% endraw %}
  </table>
</body>
```
{% endcode %}

We need to add the view and URL configuration for the `expression` URL before these changes will work. We'll do that in the next section.
