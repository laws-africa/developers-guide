---
description: Core concepts for working with Laws.Africa Knowledge Bases.
---

# Concepts

Knowledge Bases retrieve legal context from maintained Laws.Africa collections. They are designed for applications that
need search and grounding, not full local copies of legal content.

## Knowledge Base codes

Each Knowledge Base has a unique `code`. You use the code in API URLs:

```text
POST /ai/v1/knowledge-bases/{code}/retrieve
```

List the Knowledge Bases available to your account with:

```text
GET /ai/v1/knowledge-bases
```

## Places and content types

Knowledge Bases are scoped by place and content type. A place can be a country, province, municipality or other
locality.

The current content types are:

* legislation;
* judgments.

For legislation, place identifiers follow the FRBR place codes used by Akoma Ntoso and the Content API. For example,
`za` is South Africa and `za-cpt` is the City of Cape Town.

## Retrieve requests

A retrieve request includes:

* `text`: the query text to match;
* `top_k`: the maximum number of results to return;
* `filters`: optional metadata filters.

```json
{
  "text": "delict slip and trip",
  "top_k": 5,
  "filters": {}
}
```

`top_k` defaults to `10`. The maximum is `100`.

## Retrieve results

Each result includes:

* `content.text`: the matched legal text or summary;
* `metadata`: source information such as title, FRBR URI, dates and public URLs;
* `score`: an opaque relevance score.

Results are returned in descending relevance order. Higher scores indicate stronger matches. Treat scores as
opaque non-negative ranking values; compare them only within the same response.

Use public URLs from result metadata when showing results to users or grounding AI responses. They let users inspect the
source material.

## Query text

You can send a user's question directly as `text`, but AI workflows often get better matches when a model first rewrites
the question as a focused legal search query.

For example, a user may ask:

```text
Can my landlord lock me out without a court order?
```

Your application might query:

```text
eviction lockout without court order residential tenant
```

## Plans and limits

Knowledge Bases are available on the platform's Sandbox, Build and Scale plans.
Plans differ by country coverage, content availability and rate limits. See
[Pricing and plans](../get-started/pricing.md) for developer-facing details.
