---
description: Query a Knowledge Base for matching legal context.
---

# Query a Knowledge Base

Use the retrieve endpoint to query a Knowledge Base for legal information that matches keywords, phrases or AI-generated
search text.

The request must:

1. identify the Knowledge Base by `code` in the URL;
2. include the `text` to search for;
3. optionally include `top_k` and `filters`.

The response returns matching items with text, metadata and a score.

{% openapi-operation spec="laws-africa-ai-api" path="/ai/v1/knowledge-bases/{code}/retrieve" method="post" %}
[OpenAPI laws-africa-ai-api](https://api.laws.africa/ai/v1/schema)
{% endopenapi-operation %}
