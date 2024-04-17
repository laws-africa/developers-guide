---
description: Fetching the Table of Contents for an expression.
---

# Table of Contents

You can get a description of the table of contents (TOC) of a work. This includes the chapters, parts, sections and schedules that make up the legislation.

{% content-ref url="../how-to-guides/how-to-use-the-table-of-contents-api.md" %}
[how-to-use-the-table-of-contents-api.md](../how-to-guides/how-to-use-the-table-of-contents-api.md)
{% endcontent-ref %}

## Get the Table of Contents for an expression

{% swagger src="https://api.laws.africa/v3/schema" path="/v3/{frbr_uri}/toc" method="get" %}
[https://api.laws.africa/v3/schema](https://api.laws.africa/v3/schema)
{% endswagger %}

## Individual parts, chapters and sections

You can use the `url` field from an item in the Table of Contents to fetch the details of just that item in XML or HTML.

## Content for a single part, chapter or section

<mark style="color:blue;">`GET`</mark> `https://api.laws.africa/v3/:frbr-uri/:toc-item.:format`

Get the content of a particular Table of Contents item.

#### Path Parameters

| Name     | Type   | Description                           |
| -------- | ------ | ------------------------------------- |
| frbr-uri | string | The full FRBR URI for the expression. |
| toc-item | string | The Table of Contents item to fetch.  |
| format   | string | Response format: XML or HTML.         |

{% tabs %}
{% tab title="200 The HTML or XML of the requested item." %}
```markup
<section class="akn-section" id="section-9" data-id="section-9"><h3>9. The rescue of stray dogs</h3>
<section class="akn-paragraph akn--no-indent" id="section-9.paragraph-0" data-id="section-9.paragraph-0">
<span class="akn-content"><span class="akn-p">A <span class="akn-term" data-refersTo="#term-person" id="trm257" data-id="trm257">person</span> who rescues a stray <span class="akn-term" data-refersTo="#term-dog" id="trm258" data-id="trm258">dog</span> shall report the date and time of the rescue and a description of the <span class="akn-term" data-refersTo="#term-dog" id="trm259" data-id="trm259">dog</span> to the <span class="akn-term" data-refersTo="#term-Council" id="trm260" data-id="trm260">Council</span> within twenty four hours.</span></span></section></section>
```
{% endtab %}
{% endtabs %}
