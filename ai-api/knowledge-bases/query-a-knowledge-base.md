---
description: Query a Knowledge Base for matching results.
---

# Query a knowledge base

Use the **retrieve** API to query a Knowledge Base for information matching keywords or phrases.

When you query a knowledge base, you must submit a POST request:

1. Identify the Knowledge Base to query using its **code** in the URL of the request.
2. Specify the text to search the Knowledge Base for using the `text` parameter.
3. Specify filters to restrict the documents that are searched.

The API will return matching items (portions of legislation, pages of judgment text) that match your query, including a score, the text of the portion, and metadata.

## Filters

It is useful to apply filters to limit results, particularly when doing legislation searches. It is recommended you apply these filters for legislation:

* Principal works only (ie. exclude amending and commencement notices):  `principal: true`
* Exclude repealed works: `repealed: false`&#x20;

In JSON format, use:

```
"filters": {"principal": true, "repealed": false}}
```

## Example

This searches the `legislation-za` knowledge base for the text "national anthem". The filters are recommended and ensure that only un-repealed, principal Acts are returned.&#x20;

```http
POST https://api.laws.africa/ai/v1/knowledge-bases/legislation-za/retrieve
Authorization: Token abc-123
Content-Type: application/json

{
  "text": "national anthem",
  "top_k": 5,
  "filters": {"principal": true, "repealed": "false"}
}
```

The results look like this:

```
{
  "results": [
    {
      "content": {
        "text": "\n\nThe national flag of the Republic is black, gold, green, white, red and blue, as described and sketched in  Schedule 1 ."
      },
      "metadata": {
        "work_frbr_uri": "/akn/za/act/1996/constitution",
        "frbr_place": "za",
        "frbr_country": "za",
        "frbr_doctype": "act",
        "frbr_subtype": null,
        "title": "Constitution of the Republic of South Africa, 1996",
        "expression_date": "2013-08-23",
        "expression_frbr_uri": "/akn/za/act/1996/constitution/eng@2013-08-23",
        "portion_type": "provision",
        "portion_id": "chp_1__sec_5",
        "repealed": false,
        "commenced": true,
        "principal": true
      },
      "score": 0.17306449999999995
    }
  ]
}
```

## Result formats

### Legislation results

The Retrieve API for legislation Knowledge Bases returns the text of the matched provision, the score of the match (a lower score is better), and metadata for the portion. This provides very precise details on the portion of legislation that matched the query, allowing you to provide concrete citation information to your user.

The metadata includes a combination of:

1. Details of the legislative work, including [FRBR URI](../../api/works-and-expressions.md) information.
2. [Table of Contents](../../api/table-of-contents.md) information for the portion of that work.

For example:

```
{
  "metadata": {
    "work_frbr_uri": "/akn/za/act/1996/constitution",
    "frbr_place": "za",
    "frbr_country": "za",
    "frbr_doctype": "act",
    "frbr_subtype": null,
    "title": "Constitution of the Republic of South Africa, 1996",
    "expression_date": "2013-08-23",
    "expression_frbr_uri": "/akn/za/act/1996/constitution/eng@2013-08-23",
    "portion_type": "provision",
    "portion_id": "chp_1__sec_5",
    "repealed": false,
    "commenced": true,
    "principal": true
  }
}
```

#### Judgment results

The Retrieve API for judgment Knowledge Bases returns the text of the matched chunk, the score of the match (a lower score is better), and metadata for the chunk.

Up to three chunks will be returned for the same judgment.

The metadata for each chunk includes a combination of:

1. Details of the legislative work, including [FRBR URI](../../api/works-and-expressions.md) information.
2. The 1-based page number, where available.

For example:

```
{
  "metadata": {
    "work_frbr_uri": "/akn/za/judgment/zalcjhb/2014/139",
    "frbr_place": "za",
    "frbr_country": "za",
    "title": "NEHAWU obo Manyana and Another v Masege N.O. and Others (JR 363/2012) [2014] ZALCJHB 139 (8 April 2014)",
    "expression_date": "2014-04-08",
    "expression_frbr_uri": "/akn/za/judgment/zalcjhb/2014/139/eng@2014-04-08",
    "portion_type": "page",
    "portion_id": 30
  }
}
```



{% openapi-operation spec="laws-africa-ai-api" path="/ai/v1/knowledge-bases/{code}/retrieve" method="post" %}
[OpenAPI laws-africa-ai-api](https://api.laws.africa/ai/v1/schema)
{% endopenapi-operation %}
