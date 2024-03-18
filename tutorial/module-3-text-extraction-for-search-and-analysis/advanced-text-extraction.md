---
description: How to extract text from particular elements or portions of a document.
---

# Advanced text extraction

## In this section

* Extracting text from a specific section (or other portion) of a document
* Extracting text from specific elements, such as headings
* Why separating text with spaces is important

## Extracting text from a section

Sometimes it's useful to extract text only for specific portions of a document, such as a particular chapter or section.

Let's extract the text from Section 3 of the Cape Town Liquor By-law.

<figure><img src="../../.gitbook/assets/image (3).png" alt=""><figcaption><p>Section 3's HTML.</p></figcaption></figure>

Section 3's XML is below. Even a short section that looks quite simple can have complex XML.

<figure><img src="../../.gitbook/assets/image (5).png" alt=""><figcaption><p>Section 3's XML.</p></figcaption></figure>

We can use the **eId** attribute to find Section 3.

{% hint style="info" %}
The **eId** attribute is a unique identifier that appears on (almost) all elements in an Akoma Ntoso XML document. You can read more about how they are generated in the [Akoma Ntoso XML specification](https://docs.oasis-open.org/legaldocml/akn-nc/v1.0/os/akn-nc-v1.0-os.html#\_Toc531692303).&#x20;
{% endhint %}

Let's use a new xpath query to find the `<section>` element that has an `eId` of "`sec_3`".

```python
# get section 3 using its eId
sec_3 = root.xpath('//a:section[@eId="sec_3"]', namespaces=nsmap)[0]
```

The XPath query is the equivalent of the CSS selector `.section[eId=sec_3]` and means:

<table><thead><tr><th width="256">XPath</th><th>Meaning</th></tr></thead><tbody><tr><td><code>//a:section</code></td><td>Find all AKN section elements anywhere in the document</td></tr><tr><td><code>[@eId="sec_3"]</code></td><td>Filter the section elements to return only those whose <code>eId</code> attribute is <code>"sec_3"</code>.</td></tr></tbody></table>

Once you have a reference to Section 3, it is simple to extract just the text for the section using the techniques from the previous section of this module.

```python
# NOTE: ".//" not "//"
text = ' '.join(sec_3.xpath('.//text()[not(ancestor::a:remark)]', namespaces=nsmap))
text[:100]
# 3. General prohibition No  person  may  sell   liquor  to the public for on consumption or off consu
```

Note that the XPath query now starts with a dot `.//` - this is to indicate we want all text nodes starting at the current node (which is `sec_3`) and not at the root of the document. Try removing the `.` from `.//` above and see what the value of `text` is afterwards.

## Extracting text from specific elements

Sometimes it's useful to extract text only from specific elements. For example, text only in tables, chapters, headings or sub-paragraphs.

We can do this using a new XPath query. Let's extract text from only heading elements:

```python
# extract text from all headings
text = ' '.join(root.xpath('//a:heading//text()', namespaces=nsmap))
print(text)
# Definitions Application General prohibition On-consumption premises Off-consumption premises Applica
```

Note that we used `a:heading//text()` and not `a:heading/text()`. If we used `a:heading/text()` then we would not get text inside elements that are nested inside the heading, such as superscripts and subscripts inside `<sup>` and `<sub>`.

For example, consider this XML and XPath outputs.

```xml
<heading>Release of atmospheric CO<sub>2</sub>.</heading>
```

In HTML, this would be shown as: Release of atmospheric CO₂.

| XPath                 | Text                          | Comment                                                                     |
| --------------------- | ----------------------------- | --------------------------------------------------------------------------- |
| `//a:heading/text()`  | `Release of atmospheric CO.`  | Only text nodes that are **immediate children** of  `heading` are included. |
| `//a:heading//text()` | `Release of atmospheric CO2.` | All text nodes that are **descendants** of  `heading` are included.         |

Finally, let's extract the text from all headings, subheadings and cross-headings. There are two equivalent ways of doing this.

```python
# option 1 - separate options with | (meaning OR)
text = ' '.join(
  root.xpath('//a:heading//text()[not(ancestor::a:remark)]'
             '| //a:subheading//text()[not(ancestor::a:remark)]'
             '| //a:crossHeading//text()[not(ancestor::a:remark)]',
             namespaces=nsmap))

# option 2
text = ' '.join(
  root.xpath('//a:*[self::a:heading or self::a:subheading or self::a:crossHeading]'
             '//text()[not(ancestor::a:remark)]',
             namespaces=nsmap))
```

The first option joins separate XPath queries with the OR `|` operator.

The second option uses one XPath and conditions to match multiple types of elements.

## Separating text nodes with spaces

In all the examples so far, we have used `text = ' '.join(...)`. What is the `join` and why is it important?

The `' '.join(items)` takes all the elements in `items` and joins them together with a single space. It's the equivalent of the Javascript `items.join(' ')`.&#x20;

It ensures that all the text nodes are separated with a space.

Why do we need these spaces? Look what happens if we extract the text from all headings but don't join them with a space.

<pre class="language-python"><code class="lang-python"><strong># BAD: extract headings without joining with a space
</strong><strong>text = ''.join(root.xpath('//a:heading//text()', namespaces=nsmap))
</strong>print(text[:100])
# DefinitionsApplicationGeneral prohibitionOn-consumption premisesOff-consumption premisesApplication 
</code></pre>

Notice that there are spaces **within** the headings, but not **between** them.

A text node includes the spaces within the text, but does not add spaces between them. In the XML document, there are **no** spaces outside of the text nodes. All the XML is actually on one line. Only when the document is displayed by the browser as HTML, are the headings placed on lines and visual spacing is added. We must therefore add them ourselves.

{% hint style="warning" %}
It's important to separate text nodes with spaces so that we don't combine words together accidentally. This would negatively affect analysis and full-text search.
{% endhint %}
