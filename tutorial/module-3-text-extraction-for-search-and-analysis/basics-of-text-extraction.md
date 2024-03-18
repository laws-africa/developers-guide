---
description: Basic methods of extracting text from an Akoma Ntoso XML document.
---

# Basics of text extraction

## In this section

* parsing Akoma Ntoso XML using Python's LXML library
* extracting all text in a naive  way
* ignoring editorial remarks

## XML vs HTML

Extracting text from Akoma Ntoso XML is the preferred process. You can also extract text from the HTML version of an AKN document, but it's a little different because HTML is not as explicitly structured as XML.

We'll be using the [Cape Town Liquor Trading by-law](https://openbylaws.org.za/akn/za-cpt/act/by-law/2014/control-undertakings-liquor/eng@2014-12-12) for these examples.

## Parsing Akoma Ntoso XML

Let's fetch the raw XML from the Laws.Africa API.

```python
from urllib.request import Request, urlopen

TOKEN = "your-auth-token"
url = "https://api.laws.africa/v3/akn/za-cpt/act/by-law/2014/control-undertakings-liquor/eng/.xml"
request = Request(url, headers={"Authorization": f"Token {TOKEN}"})
# raw_xml is a bytes object, not a str
raw_xml = urlopen(request).read()
print(raw_xml[:100])
# b'<akomaNtoso xmlns="http://docs.oasis-open.org/legaldocml/ns/akn/3.0"><act ...
```

Now, parse the bytes in `raw_xml` into an XML tree with lxml.

```python
from lxml import etree
# parse the raw XML bytes (even though the method name is "fromstring")
root = etree.fromstring(raw_xml)
```

## Extracting all text content (naive approach)

If you look at the XML file in a browser (which formats it), you'll see that the XML contains a mixture of metadata, structure and text content.

<figure><img src="../../.gitbook/assets/image (2).png" alt=""><figcaption><p>Example of Akoma Ntoso XML, including metadata and text.</p></figcaption></figure>

The simplest way of extracting text is to ignore the structure and just extract all text nodes.

<pre class="language-python"><code class="lang-python"><strong># itertext() iterates over all text nodes in the entire document
</strong><strong>text = ' '.join(root.itertext())
</strong>print(text[:100])
# To provide for the control of undertakings selling liquor to the public including the control of tra
</code></pre>

This is quick and easy, but includes **all** text nodes, which may not be what we want:

* It includes text from all structural elements including headings, numbers, editorial comments, footnotes, quoted and embedded elements, tables, etc.
* It includes all portions of the document, including attachments such as schedules and appendixes.

## Ignoring editorial remarks

When legislation is edited or amended, sometimes editorial remarks are added. For example:

<pre class="language-xml"><code class="lang-xml">&#x3C;p eId="sec_6__subsec_4__p_1">
<strong>  &#x3C;remark status="editorial">
</strong><strong>    [subsection (4) deleted by section 1 of the &#x3C;ref href="/akn/za-cpt/act/by-law/2014/control-undertakings-liquor-amendment" eId="sec_6__subsec_4__p_1__ref_1">Amendment By-law, 2014&#x3C;/ref>]
</strong>  &#x3C;/remark>
&#x3C;/p>
</code></pre>

These are not substantive and not officially part of the legal text of the document. We usually want to exclude these remarks when extracting text for full-text search purposes.

To do this, we can use [XPath](https://en.wikipedia.org/wiki/XPath), a powerful mechanism of querying elements in XML documents.

{% hint style="info" %}
Learn more about XPath at these resources:

* [Zyte's XPath tutorial for web scraping](https://www.zyte.com/blog/an-introduction-to-xpath-with-examples/)
* [Using lxml and XPath to extract text in Python](https://lxml.de/tutorial.html#using-xpath-to-find-text)&#x20;
{% endhint %}

XPath is rich and expressive and we will only cover the basics for this example. There are two key ideas we need:

* A **namespace** lets us ignore parts of the document that aren't part of the Akoma Ntoso XML standard. The [official Akoma Ntoso XML namespace](https://docs.oasis-open.org/legaldocml/akn-core/v1.0/akn-core-v1.0-part2-specs.html) is `http://docs.oasis-open.org/legaldocml/ns/akn/3.0`&#x20;
* The **query** which describes the elements we want from the XML document. In this case, our query will mean "text nodes that aren't part of `<remark>` elements".

```python
# The AKN namespace is the default one for this XML document,
# which is given by the None entry in the namespace map (nsmap).
# This is the same as ns = "http://docs.oasis-open.org/legaldocml/ns/akn/3.0"
ns = root.nsmap[None]

# This tells xpath that when we use the "a" alias, we mean the AKN namespace.
# It saves us from writing the full namespace in the xpath query.
nsmap = {"a": ns}

# query all the text nodes that don't have <remark> as an ancestor
text = ' '.join(root.xpath("//text()[not(ancestor::a:remark)]", namespaces=nsmap))
print(text[:100])
# To provide for the control of undertakings selling liquor to the public including the control of tra
```

Here's a brief explanation of what the components of the XPath query mean:

<table><thead><tr><th width="258">XPath</th><th>Meaning</th></tr></thead><tbody><tr><td><code>//</code></td><td>Any node at any point in the tree. Alternatively, <code>./</code> means nodes at the current element (which is <code>root</code> in this case), or <code>/</code> which means the root of the entire document.</td></tr><tr><td><code>text()</code></td><td>This matches text nodes.</td></tr><tr><td><code>[...]</code></td><td>This applies additional conditions to the text nodes being matched. The conditions must evaluate to true to be included in the resulting node set.</td></tr><tr><td><code>not(...)</code></td><td>This negates the condition inside the brackets.</td></tr><tr><td><code>ancestor::a:remark</code></td><td>The <code>ancestor::</code> means any ancestor element of the text node, and <code>a:remark</code> limits the ancestors to those that are <code>&#x3C;remark></code> elements in the Akoma Ntoso namespace.</td></tr></tbody></table>

## Ignoring other content

Other types of text content you may want to ignore, depending on your use case:

* Text of headings and sub-headings: `<heading>`, `<subheading>`, `<crossHeading>`
* The numbers of chapters, sections, etc.: `<num>`
* Quoted and embedded content: `<quotedStructure>`, `<embeddedStructure>`

Depending on your needs, you can adjust the XPath to include these additional elements by including them in the `not(...)` clause and separating them with `or`.

```python
# get text that isn't in a remark, heading or num
text = ' '.join(root.xpath(
  "//text()[not(ancestor::a:remark or ancestor::a:num or ancestor::a:heading)]",
  namespaces=nsmap))
```

In the next section, we'll explore how to extract text only for certain portions of the document, such as a particular section or chapter, or only table elements.
