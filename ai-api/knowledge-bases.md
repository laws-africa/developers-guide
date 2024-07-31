---
description: Use knowledge bases to find legal information related to a query.
---

# Knowledge Bases

## What are Knowledge Bases?

A Knowledge Base (KB) is a database of legal information stored using an embedding database. You can use the **retrieve** API to query the Knowledge Base for items of legal data that match a query.

Our legislation Knowledge Bases contain the full text of all the digitised legislation that is available through the [Laws.Africa Content API](../api/about-the-api.md). The legislation is split into portions (chapters, sections, paragraphs, etc.) and embeddings are calculated for each portion. These embeddings make it possible to perform semantic queries against the data to find portions relevant to a keyword, phrase or question.

{% hint style="info" %}
Knowledge Bases include only the most recent version of legislation. See [Works and Expressions](../get-started/works-and-expressions.md) for more details on how legislation changes over time.
{% endhint %}

## List available Knowledge Bases

{% swagger src="../.gitbook/assets/Laws.Africa AI API (v1).yaml" path="/ai/v1/knowledge-bases" method="get" %}
[Laws.Africa AI API (v1).yaml](<../.gitbook/assets/Laws.Africa AI API (v1).yaml>)
{% endswagger %}

## Get details of a single knowledge base

{% swagger src="../.gitbook/assets/Laws.Africa AI API (v1).yaml" path="/ai/v1/knowledge-bases/{code}" method="get" %}
[Laws.Africa AI API (v1).yaml](<../.gitbook/assets/Laws.Africa AI API (v1).yaml>)
{% endswagger %}

## Query a Knowledge Base

Use the **retrieve** API to query a Knowledge Base for information matching keywords or phrases.

When you query a knowledge base, you must submit a POST request:

1. Identify the Knowledge Base to query using its **code** in the URL of the request.
2. Specify the text to search the Knowledge Base for using the `text` parameter.

The API will return matching items (portions of legislation) that match your query, including a score, the text of the portion, and metadata.

{% swagger src="../.gitbook/assets/Laws.Africa AI API (v1).yaml" path="/ai/v1/knowledge-bases/{code}/retrieve" method="post" %}
[Laws.Africa AI API (v1).yaml](<../.gitbook/assets/Laws.Africa AI API (v1).yaml>)
{% endswagger %}

### Result item information

The API returns the text of the matching item, the score from 0 (a very poor match) to 1.0 (a very good match), and metadata for the portion. This provides very precise details on the portion of legislation that matched the query, allowing you to provide concrete citation information to your user.

The metadata includes a combination of:

1. Details of the legislative work, including [FRBR URI](../api/works-and-expressions.md) information.
2. [Table of Contents](../api/table-of-contents.md) information for the portion of that work.

For example:

```
{
  "metadata": {
    "portion_id": "chp_2__sec_2",
    "frbr_locality": "jhb",
    "portion_num": "2.",
    "portion_title": "2. Restriction on number of dogs",
    "frbr_place": "za-jhb",
    "title": "Dogs and Cats",
    "expression_date": "2019-09-04",
    "portion_frbr_uri": "/akn/za-jhb/act/by-law/2006/dogs-and-cats/eng/~chp_2__sec_2",
    "work_frbr_uri": "/akn/za-jhb/act/by-law/2006/dogs-and-cats",
    "portion_heading": "Restriction on number of dogs",
    "frbr_country": "za",
    "portion_type": "section"
  }
}
```

&#x20;

