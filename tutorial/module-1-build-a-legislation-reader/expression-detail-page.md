---
description: Add a document detail page.
---

# Expression detail page

## In this section

* Displaying the details and content of an expression

## Expression detail page

Now let's add an expression detail page that shows the content of a single expression of a work.

Add a new view to your `views.py`:

{% code title="views.py" %}
```python
from django.views.generic import ListView, DetailView
from .models import Work, Expression

class WorkListView(ListView):
    model = Work
    
class ExpressionDetailView(DetailView):
    model = Expression
    slug_field = 'frbr_uri'
    slug_url_kwarg = 'frbr_uri'
```
{% endcode %}

Add the new view to your `urls.py`:

{% code title="urls.py" %}
```python
from django.urls import path
from reader.views import *

urlpatterns = [
    path('', WorkListView.as_view(), name="home"),
    # the FRBR URI starts with a /
    path('expression<path:frbr_uri>', ExpressionDetailView.as_view(), name="expression"),
]
```
{% endcode %}

Now create a template called `templates/reader/expression_detail.html`. We'll start with just the title and the expression content.

{% code title="expression_detail.html" %}
```html
<html>
<head>
  <title>{{ expression.title }}</title>
</head>
<body>
  <a href="{% raw %}
{% url 'home' %}
{% endraw %}">Home</a>
  
  <h1>{{ expression.title }}</h1>
  
  <div>{{ expression.content|safe }}</div>
</body>
```
{% endcode %}

{% hint style="info" %}
If there are other expressions on the same work (different language or date), how would the user find them?
{% endhint %}

If you visit an expression detail page, you'll see the title and some ugly content. In the next section we'll add formatting to the content.
