---
description: Make your first Laws.Africa Knowledge Base query.
---

# Quick start

This guide shows you how to query a Knowledge Base and use the result in an application.

You will:

1. create a platform account and API token;
2. list available Knowledge Bases;
3. query a Knowledge Base;
4. use the returned legal context in Python.

## Create an API token

1. Sign up at [https://platform.laws.africa/](https://platform.laws.africa/).
2. Get your API token from [https://platform.laws.africa/api-keys/](https://platform.laws.africa/api-keys/).

In the examples below, replace `<YOUR_AUTH_TOKEN>` with your token.

## List available Knowledge Bases

```bash
curl -H "Authorization: Token <YOUR_AUTH_TOKEN>" \
  https://api.laws.africa/ai/v1/knowledge-bases
```

The response includes Knowledge Base codes. Use a code in the retrieve endpoint.

## Query a legislation Knowledge Base

This example queries the South African municipal legislation Knowledge Base for
Cape Town dog ownership rules.

```bash
curl -X POST \
  -H "Authorization: Token <YOUR_AUTH_TOKEN>" \
  -H "Content-Type: application/json" \
  -d '{
    "text": "dog ownership cape town",
    "top_k": 5,
    "filters": {
      "principal": true,
      "repealed": false,
      "frbr_place": "za-cpt"
    }
  }' \
  https://api.laws.africa/ai/v1/knowledge-bases/legislation-za-municipal/retrieve
```

The response contains matching legal portions:

```json
{
  "results": [
    {
      "content": {
        "text": "..."
      },
      "metadata": {
        "title": "Animal By-law, 2011",
        "work_frbr_uri": "/akn/za-cpt/act/by-law/2011/animal",
        "public_url": "https://lawlibrary.org.za/...",
        "portion_title": "Chapter 7 - Miscellaneous",
        "portion_public_url": "https://lawlibrary.org.za/...#chp_7"
      },
      "score": 0.16151309999999997
    }
  ]
}
```

{% hint style="info" %}
For legislation queries, start with `principal: true` and `repealed: false` so results prefer current principal
legislation rather than amendment notices or repealed works.
{% endhint %}

## Use the result in Python

```python
import requests

TOKEN = "<YOUR_AUTH_TOKEN>"
KB_CODE = "legislation-za-municipal"

response = requests.post(
    f"https://api.laws.africa/ai/v1/knowledge-bases/{KB_CODE}/retrieve",
    headers={
        "Authorization": f"Token {TOKEN}",
        "Content-Type": "application/json",
    },
    json={
        "text": "dog ownership cape town",
        "top_k": 5,
        "filters": {
            "principal": True,
            "repealed": False,
            "frbr_place": "za-cpt",
        },
    },
    timeout=30,
)
response.raise_for_status()

results = response.json()["results"]

context = []
for result in results:
    metadata = result["metadata"]
    context.append(
        "\n".join(
            [
                f"Title: {metadata.get('title')}",
                f"Source: {metadata.get('portion_public_url') or metadata.get('public_url')}",
                f"Text: {result['content']['text']}",
            ]
        )
    )

print("\n\n".join(context))
```

Pass this context into your search interface, RAG prompt or agent response with the user's question. Always include
source URLs so users can inspect the legal material.

## Next steps

* Learn the main [Knowledge Base concepts](concepts.md).
* Apply [filters](filters.md) for more precise legislation results.
* Use results in [RAG, search and agent workflows](use-in-apps.md).
