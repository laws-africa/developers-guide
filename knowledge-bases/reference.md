---
description: Knowledge Base API reference.
---

# API Reference

Knowledge Base endpoints are available under:

{% code collapsedlinecount="10" %}
```
https://api.laws.africa/ai/v1/knowledge-bases
```
{% endcode %}

Use these endpoints to list available Knowledge Bases, inspect a single Knowledge Base and retrieve matching legal context.

## List Knowledge Bases

Call this endpoint to list the Knowledge Bases available to your account.

{% openapi-operation spec="laws-africa-ai-api" path="/ai/v1/knowledge-bases" method="get" %}
[OpenAPI laws-africa-ai-api](https://api.laws.africa/ai/v1/schema)
{% endopenapi-operation %}

## Get a Knowledge Base

Get details for a Knowledge Base using its unique code.

{% openapi-operation spec="laws-africa-ai-api" path="/ai/v1/knowledge-bases/{code}" method="get" %}
[OpenAPI laws-africa-ai-api](https://api.laws.africa/ai/v1/schema)
{% endopenapi-operation %}

## Query a Knowledge Base

Use the retrieve endpoint to query a Knowledge Base for legal information that matches keywords, phrases or AI-generated search text.

The request must:

1. identify the Knowledge Base by `code` in the URL;
2. include the `text` to search for;
3. optionally include `top_k` and `filters`.

The response returns matching items with text, metadata and a score.

{% openapi-operation spec="laws-africa-ai-api" path="/ai/v1/knowledge-bases/{code}/retrieve" method="post" %}
[OpenAPI laws-africa-ai-api](https://api.laws.africa/ai/v1/schema)
{% endopenapi-operation %}
