---
description: Fetching the Table of Contents for an expression.
---

# Table of Contents

You can get a description of the table of contents (TOC) of a work. This includes the chapters, parts, sections and schedules that make up the legislation, based on the structure captured by the Laws.Africa editor.

{% content-ref url="../how-to-guides/how-to-use-the-table-of-contents-api.md" %}
[how-to-use-the-table-of-contents-api.md](../how-to-guides/how-to-use-the-table-of-contents-api.md)
{% endcontent-ref %}

Each item in the table of contents has this structure:

```javascript
{
  "id": "chapter-1",
  "type": "chapter",
  "num": "1",
  "heading": "Interpretation",
  "title": "Chapter 1 - Interpretation",
  "component": "main",
  "subcomponent": "chapter/1",
  "url": "http://api.laws.africa/v2/akn/za/act/1998/10/eng/main/chapter/1",
  "children": [ "..." ]
}
```

Each of these fields is described in the table below.

| Field        | Description                                                                                                                                         | Type   |
| ------------ | --------------------------------------------------------------------------------------------------------------------------------------------------- | ------ |
| id           | The unique XML element id of this item. (optional)                                                                                                  | String |
| type         | The Akoma Ntoso element name of this item.                                                                                                          | String |
| num          | The number of this item, such as a chapter, part or section number. (optional)                                                                      | String |
| heading      | The heading of this item (optional)                                                                                                                 | String |
| title        | A derived, friendly title of this item, taking `num` and `heading` into account and providing good defaults if either of those is missing.          | String |
| component    | The component of the Akoma Ntoso document that this item is a part of, such as `main` for the main document, or `schedule1` for the first schedule. | String |
| subcomponent | The subcomponent of the component that this item is a part of, such as a chapter. (optional)                                                        | String |
| url          | The API URL for this item, which can be used to fetch XML, HTML and other details of just this part of the document.                                | String |
| children     | A possibly-empty array of TOC items that are children of this item.                                                                                 | Arra   |

{% swagger baseUrl="https://api.laws.africa" path="/v2/:frbr-uri/toc.json" method="get" summary="Table of  contents for an expression" %}
{% swagger-description %}
Gets the Table of Contents for a particular expression in JSON format.
{% endswagger-description %}

{% swagger-parameter in="path" name="frbr-uri" type="string" %}
Full FRBR URI for the work
{% endswagger-parameter %}

{% swagger-parameter in="header" name="Authorization" type="string" %}
Authentication token to identify you.
{% endswagger-parameter %}

{% swagger-response status="200" description="The Table of Contents for this expression, in JSON format." %}
```javascript
{
  "toc": [
    {
      "type": "preface",
      "component": "main",
      "subcomponent": "preface",
      "title": "Preface",
      "url": "https://api.laws.africa/v2/akn/na/act/2001/16/eng@2005-03-01/main/preface"
    },
    {
      "type": "section",
      "component": "main",
      "subcomponent": "section/1",
      "title": "1. Definitions",
      "heading": "Definitions",
      "num": "1.",
      "id": "section-1",
      "url": "https://api.laws.africa/v2/akn/na/act/2001/16/eng@2005-03-01/main/section/1"
    },
  ]
}
```
{% endswagger-response %}
{% endswagger %}

## Individual parts, chapters and sections

You can use the `url` field from an item in the Table of Contents to fetch the details of just that item in XML or HTML.

{% swagger baseUrl="https://api.laws.africa" path="/v2/:frbr-uri/:toc-item.:format" method="get" summary="Content for a single part, chapter or section" %}
{% swagger-description %}
Get the content of a particular Table of Contents item.
{% endswagger-description %}

{% swagger-parameter in="path" name="frbr-uri" type="string" %}
The full FRBR URI for the expression.
{% endswagger-parameter %}

{% swagger-parameter in="path" name="toc-item" type="string" %}
The Table of Contents item to fetch.
{% endswagger-parameter %}

{% swagger-parameter in="path" name="format" type="string" %}
Response format: XML or HTML.
{% endswagger-parameter %}

{% swagger-response status="200" description="The HTML or XML of the requested item." %}
```markup
<section class="akn-section" id="section-9" data-id="section-9"><h3>9. The rescue of stray dogs</h3>
<section class="akn-paragraph akn--no-indent" id="section-9.paragraph-0" data-id="section-9.paragraph-0">
<span class="akn-content"><span class="akn-p">A <span class="akn-term" data-refersTo="#term-person" id="trm257" data-id="trm257">person</span> who rescues a stray <span class="akn-term" data-refersTo="#term-dog" id="trm258" data-id="trm258">dog</span> shall report the date and time of the rescue and a description of the <span class="akn-term" data-refersTo="#term-dog" id="trm259" data-id="trm259">dog</span> to the <span class="akn-term" data-refersTo="#term-Council" id="trm260" data-id="trm260">Council</span> within twenty four hours.</span></span></section></section>
```
{% endswagger-response %}
{% endswagger %}
