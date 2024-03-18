---
description: Adding some basic interactivity using Law Widgets.
---

# Adding interactivity

## In this section

* Add a searchable Table of Contents using Law Widgets
* Add interactivity with Law Widgets for defined terms and internal references

Now that we have a detail page showing the content of the documents, let's add some interactivity using Laws.Africa's [Law Widgets](https://github.com/laws-africa/law-widgets).

## Table of Contents

A searchable Table of Contents is important to make long legislative documents easier to navigate. There are two widgets you can use for the Table of Contents:

* [\<la-table-of-contents>](https://github.com/laws-africa/law-widgets/blob/main/core/src/components/table-of-contents/readme.md) - this is a basic Table of Contents tree.
* [\<la-table-of-contents-controller>](https://github.com/laws-africa/law-widgets/blob/main/core/src/components/table-of-contents-controller/readme.md) - this is a fully searchable and collapsible Table of Contents tree that builds on top of \<la-table-of-contents>.

We'll use la-table-of-contents-controller because it gives us rich functionality right out the box.

Let's divide our content section into two columns to make room for the Table of Contents in a sidebar on the left of the page.

{% code title="expression_detail.html" %}
```html
<html>
<head>
  <title>{{ expression.title }}</title>
  <!-- add the law widgets javascript -->
  <script
    type="module"
    src="https://cdn.jsdelivr.net/npm/@lawsafrica/law-widgets@latest/dist/lawwidgets/lawwidgets.esm.js"
  ></script>
</head>
<body>
  <a href="{% raw %}
{% url 'home' %}
{% endraw %}">Home</a>
  
  <h1>{{ expression.title }}</h1>
  
  <div style="display: flex">
    <aside style="flex: 1">
      <la-table-of-contents-controller
        items="{{ toc_json }}"
        style="position: sticky; top: 0; max-height: 100vh; overflow-y: auto;"
      ></la-table-of-contents-controller>
    </aside>
    
    <div style="flex: 3">
      <!-- use la-akoma-ntoso law widget to apply styles -->
      <la-akoma-ntoso frbr-expression-uri="{{ expression.frbr_uri }}">
        {{ expression.content|safe }}
      </la-akoma-ntoso>
    </div>
  </div>
</body>
```
{% endcode %}

We need to provide the `<la-table-of-contents-controller>` with the JSON Table of Contents data in the items attribute. This is the data that is provided by the Laws.Africa Content API.

Update the `views.py` to add turn the TOC information into JSON.

{% code title="views.py" %}
```python
import json

# ...

class ExpressionDetailView(DetailView):
    model = Expression
    slug_field = 'frbr_uri'
    slug_url_kwarg = 'frbr_uri'

    def get_context_data(self, **kwargs):
        context = super().get_context_data(**kwargs)
        context['toc_json'] = json.dumps(self.object.toc_json['toc'])
        return context
```
{% endcode %}

The page now has an interactive table of contents.

## Popups for defined terms

We're going to use the [\<la-decorate-terms>](https://github.com/laws-africa/law-widgets/blob/main/core/src/components/decorate-terms/readme.md) Law Widget to add popups for defined terms that occur in a document.

<figure><img src="../../.gitbook/assets/image (7).png" alt=""><figcaption></figcaption></figure>

Modify `expression_detail.html` to include the new widget anywhere on the page:

<pre class="language-html" data-title="expression_detail.html"><code class="lang-html">...
<strong>&#x3C;la-decorate-terms popup-definitions link-terms>&#x3C;/la-decorate-terms>
</strong>&#x3C;la-akoma-ntoso ...>...&#x3C;/la-akoma-ntoso>
...
</code></pre>

This widget will add term definition popups and links in the first `<la-akoma-ntoso>` element on the page. If there is more than one, you can target it specifically by providing an **akoma-ntoso** attribute that uses a CSS selector to identify the target element: `<la-decorate-terms akoma-ntoso="#akn-doc">`.

You can control the behaviour by adding or removing these attributes. Try changing them and refreshing the page to see what changes.

* `popup-definitions` - controls whether definitions of terms are shown in popups
* `link-terms` - controls whether references to terms are shown as clickable links.

## Popups for internal references

We're going to use the [\<la-decorate-internal-refs>](https://github.com/laws-africa/law-widgets/blob/main/core/src/components/decorate-internal-refs/readme.md) widget to make internal references more interactive. The widget will show a purple bookmark at an internal reference, and show the content of the target element when the user hovers over the reference.

<figure><img src="../../.gitbook/assets/image (6).png" alt=""><figcaption></figcaption></figure>

Modify `expression_detail.html` to include the new widget:

{% code title="expression_detail.html" %}
```html
...
<la-decorate-internal-refs popups flag></la-decorate-internal-refs>
<la-akoma-ntoso ...>...</la-akoma-ntoso>
...
```
{% endcode %}

This widget will add interactivity to internal references in the first `<la-akoma-ntoso>` element on the page. Like with `<la-decorate-terms>` , you can use the **akoma-ntoso** attribute to target a specific `<la-akoma-ntoso>` element using a CSS selector, such as `<la-decorate-internal-refs akoma-ntoso="#akn-doc">`.

You can control the behaviour by adding or removing these attributes. Try changing them and refreshing the page to see what changes.

* `popups` - controls whether reference target elements are shown in popups
* `flag` - controls whether internal references are decorated with a purple bookmark icon
