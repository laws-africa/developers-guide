---
description: Choose the Laws.Africa API that matches your product.
---

# Choose Knowledge Bases or the Content API

Start with the API that matches what your product needs to do.

## When to use Knowledge Bases

Use Knowledge Bases when your product needs to find relevant legal context.

Knowledge Bases are best for:

* legal AI assistants;
* RAG systems;
* legal agents and tool calls;
* semantic legal search;
* workflow tools that need relevant legislation or case law;
* prototypes where you do not want to build ingestion and indexing pipelines.

With Knowledge Bases, Laws.Africa maintains the source collections, indexes and
retrieval layer. Your application sends a search query and gets back matching
legal content with metadata and public source URLs.

{% content-ref url="../knowledge-bases/quick-start.md" %}
[quick-start.md](../knowledge-bases/quick-start.md)
{% endcontent-ref %}

## When to use the Content API

Use the Content API when your product needs full legislation content inside your own systems.

The Content API is best for:

* legal publishing systems;
* compliance infrastructure;
* internal legal databases;
* offline processing;
* analytics and classification pipelines;
* products that need XML, HTML, PDF or point-in-time legislation versions.

With the Content API, your system controls storage, processing and presentation.
You can use webhooks to receive update notifications for subscribed legislation
content.

{% content-ref url="../content-api/quick-start.md" %}
[quick-start.md](../content-api/quick-start.md)
{% endcontent-ref %}

## When to use both

Some products use both APIs:

1. Use Knowledge Bases to find relevant legislation or judgments for a user's query.
2. Use the Content API to fetch full legislation content when the product needs
   to render, store or process the complete document.

This is useful when a product starts as search or AI grounding, but later needs deeper content control.

## How pricing affects the choice

Knowledge Bases are available from the free Sandbox plan, which makes them the
lowest-friction starting point for evaluation and prototypes.

Full Content API access is available on Scale and Enterprise plans. Use it when
your product needs complete legislation collections, historical versions,
structured formats or update webhooks.

{% content-ref url="pricing.md" %}
[pricing.md](pricing.md)
{% endcontent-ref %}
