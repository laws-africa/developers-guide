---
description: Retrieve authoritative African legal context for AI, search and workflow tools.
---

# Knowledge Bases

A Knowledge Base is a searchable legal collection for a place and content type,
such as national legislation, municipal by-laws or court judgments.

Use Knowledge Bases when your application needs relevant legal context but does
not need to store and maintain full legal collections itself. They are the
fastest way to build legal AI assistants, RAG systems, legal agents, semantic
search and workflow tools grounded in African legal information.

When you query a Knowledge Base, Laws.Africa runs text and semantic search over
maintained legal collections and returns the best matching results with:

* legal text or summaries;
* source metadata;
* public URLs for inspection and citation;
* a match score.

## What you can build

Knowledge Bases are useful for:

* grounding legal AI answers in maintained sources;
* retrieving legal context for RAG pipelines;
* powering semantic legislation or judgment search;
* giving agents a legal retrieval tool;
* triaging legal research questions before deeper review.

## Start here

{% content-ref url="quick-start.md" %}
[quick-start.md](quick-start.md)
{% endcontent-ref %}

{% content-ref url="concepts.md" %}
[concepts.md](concepts.md)
{% endcontent-ref %}

{% content-ref url="use-in-apps.md" %}
[use-in-apps.md](use-in-apps.md)
{% endcontent-ref %}

## API endpoint

Knowledge Bases are available at:

```text
https://api.laws.africa/ai/v1/knowledge-bases
```

The Knowledge Base retrieve endpoint is:

```text
POST https://api.laws.africa/ai/v1/knowledge-bases/{code}/retrieve
```

See the [reference](reference/README.md) for endpoint details.
