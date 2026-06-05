---
description: Start building with the Laws.Africa Legal Knowledge Platform.
---

# Platform overview

The Laws.Africa Legal Knowledge Platform helps developers build products that
use authoritative African legal information.

The platform has two complementary APIs:

* **Knowledge Bases** retrieve relevant legislation and judgment context for AI
  grounding, RAG, legal agents, search and workflow tools.
* **Content API** provides full legislation content and metadata in structured formats for deeper integrations.

Most new integrations should start with Knowledge Bases. They are easier to
prototype with because Laws.Africa handles ingestion, indexing, embeddings and
updates. Your application sends a query and receives relevant legal context with
metadata and public source links.

Use the Content API when you need direct access to full legislation collections
inside your own infrastructure.

## How the products fit together

Knowledge Bases are for retrieval. They answer the question: "What legal material is relevant to this query?"

The Content API is for content access. It answers the question: "Give me this
legislation, in this format, so I can store, render or process it myself."

## How to get started

### Prototype with Knowledge Bases

1. Create a free platform account.
2. Get an API token.
3. Query a Knowledge Base with a legal question or search phrase.
4. Use the returned text, metadata and public URLs in your app, search interface
   or AI workflow.

{% content-ref url="../knowledge-bases/quick-start.md" %}
[quick-start.md](../knowledge-bases/quick-start.md)
{% endcontent-ref %}

### Build a deeper Content API integration

1. Choose the country or locality you need.
2. Fetch works and expressions.
3. Store the legislation and metadata you need.
4. Use webhooks or polling to keep your copy up to date.

{% content-ref url="../content-api/quick-start.md" %}
[quick-start.md](../content-api/quick-start.md)
{% endcontent-ref %}

## Plans and access

The membership plans support a progression from experimentation to production:

* **Sandbox** for free experimentation with one country and limited daily usage.
* **Build** for production Knowledge Base workflows in one country.
* **Scale** for broader coverage, higher usage and full Content API access.

See [pricing and plans](pricing.md) for developer-facing plan details, limits
and current pricing guidance.
