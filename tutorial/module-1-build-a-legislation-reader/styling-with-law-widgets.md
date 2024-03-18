---
description: Adding styles to the document content.
---

# Styling with Law Widgets

## In this section

* Installing the Law Widgets web components library
* Using Law Widgets to add styles to document content
* Customising styles with CSS

## Law Widgets

Akoma Ntoso HTML documents don't look very good without styling. Laws.Africa's [Law Widgets](https://github.com/laws-africa/law-widgets/blob/main/core/README.md) is a collection of web components that make it simple to style the HTML provided by the Laws.Africa Content API.

Using Law Widgets is easy:

1. Add a reference to the Law Widgets javascript.
2. Use the law widgets in your HTML.

We'll use the [\<la-akoma-ntoso>](https://github.com/laws-africa/law-widgets/blob/main/core/src/components/akoma-ntoso/readme.md) widget to apply appropriate styles to the expression HTML in `expression_detail.html`.

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
  
  <!-- use la-akoma-ntoso law widget to apply styles -->
  <la-akoma-ntoso frbr-expression-uri="{{ expression.frbr_uri }}">
    {{ expression.content|safe }}
  </la-akoma-ntoso>
</body>
```
{% endcode %}

If you refresh your expression page in your browser, you should now have a well-styled document.

<figure><img src="../../.gitbook/assets/image (8).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
You can also work directly with the Law Widget SCSS styles without using Law Widgets. See [https://github.com/laws-africa/law-widgets/tree/main/law-widget-styles](https://github.com/laws-africa/law-widgets/tree/main/law-widget-styles) for more details.&#x20;
{% endhint %}

### The frbr-expression-uri attribute

We included the **frbr-expression-uri** attribute on the `<la-akoma-ntoso>` element. If you inspect the `<la-akoma-ntoso>` element in your browser's inspector, you'll see that new attributes have been added automatically:

```html
<la-akoma-ntoso
  frbr-expression-uri="/akn/za-cpt/act/by-law/2014/control-undertakings-liquor/eng@2014-12-12"
  frbr-country="za"
  frbr-type="act"
  frbr-subtype="by-law"
  frbr-date="2014"
  frbr-number="control-undertakings-liquor"
  frbr-expression-date="2014-12-12"
  frbr-language="eng"
>...</la-akoma-ntoso>
```

These additional attributes are derived from the Expression FRBR URI. They allows Law Widgets (and you) to customise the applied styles depending on a country's legal tradition.

For example, in Namibia's legal tradition editorial comments are <mark style="color:green;">**green**</mark> and **bold**. This styling is applied based on CSS selectors that work on the FRBR attributes added above.

{% hint style="info" %}
The different legal tradition styles that Law Widgets supports are in the [traditions folder in the GitHub repo](https://github.com/laws-africa/law-widgets/tree/main/law-widget-styles/scss/mixins/traditions).&#x20;
{% endhint %}

## Customising styles

You can easily apply your own CSS styles to the Akoma Ntoso HTML using regular CSS selectors.

For example, we can add a red border to all sections, except for Kenya (country code KE) where we can add a green border:

```css
la-akoma-ntoso .akn-section {
  border: 1px solid red;
}
la-akoma-ntoso[frbr-country=ke] .akn-section {
  border-color: green;
}
```

You can use different attribute-based CSS selectors to change styling for different document types, countries, localities and languages.
