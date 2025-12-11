---
description: Use knowledge bases to find legal information related to a query.
---

# Knowledge Bases

## What are Knowledge Bases?

A Knowledge Base (KB) is a database of legal information. You can use the **retrieve** API to query the Knowledge Base for items of legal data that match a query, such as legislation and court judgments. You can then use the returned information in your application, such as in a [RAG](https://en.wikipedia.org/wiki/Retrieval-augmented_generation) or LLM agent workflow.

When you use the retrieve API to query the Knowledge Base, Laws.Africa performs a text and semantic search for the best portions of documents that match the query, and returns those portions and related metadata.

Knowledge Bases contain information for a particular region, such as a national, provincial or municipal level.

Explore the documentation to learn how to integrate Knowledge Bases into your application.

{% content-ref url="types-of-knowledge-bases.md" %}
[types-of-knowledge-bases.md](types-of-knowledge-bases.md)
{% endcontent-ref %}

{% content-ref url="using-a-knowledge-base.md" %}
[using-a-knowledge-base.md](using-a-knowledge-base.md)
{% endcontent-ref %}

{% content-ref url="query-a-knowledge-base.md" %}
[query-a-knowledge-base.md](query-a-knowledge-base.md)
{% endcontent-ref %}
