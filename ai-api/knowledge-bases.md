---
description: Use knowledge bases to find legal information related to a query.
---

# Knowledge Bases

## What are Knowledge Bases?

A Knowledge Base (KB) is a database of legal information stored using an embedding database. You can use the **retrieve** API to query the Knowledge Base for items of legal data that match a query.

Our legislation Knowledge Bases contain the full text of all the digitised legislation that is available through the [Laws.Africa Content API](../api/about-the-api.md). The legislation is split into portions (chapters, sections, paragraphs, etc.) and embeddings are calculated for each portion. These embeddings make it possible to perform semantic queries against the data to find portions relevant to a keyword, phrase or question.

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

{% swagger src="../.gitbook/assets/Laws.Africa AI API (v1).yaml" path="/ai/v1/knowledge-bases/{code}/retrieve" method="post" %}
[Laws.Africa AI API (v1).yaml](<../.gitbook/assets/Laws.Africa AI API (v1).yaml>)
{% endswagger %}
