---
description: Query a Knowledge Base for matching results.
---

# Query a Knowledge Base

Use the **retrieve** API to query a Knowledge Base for information matching keywords or phrases.

When you query a knowledge base, you must submit a POST request:

1. Identify the Knowledge Base to query using its **code** in the URL of the request.
2. Specify the text to search the Knowledge Base for using the `text` parameter.
3. Specify filters to restrict the documents that are searched.

The API will return matching items (portions of legislation, judgment summaries) that match your query, including a score, the text of the portion, and metadata.

{% openapi-operation spec="laws-africa-ai-api" path="/ai/v1/knowledge-bases/{code}/retrieve" method="post" %}
[OpenAPI laws-africa-ai-api](https://api.laws.africa/ai/v1/schema)
{% endopenapi-operation %}
