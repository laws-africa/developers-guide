---
description: Use knowledge bases to find legal information related to a query.
---

# Knowledge Bases

## What are Knowledge Bases?

A Knowledge Base (KB) is a database of legal information stored using an embedding database. You can use the **retrieve** API to query the Knowledge Base for items of legal data that match a query.

### Legislation Knowledge Bases

Our legislation Knowledge Bases contain the full text of all the digitised legislation that is available through the [Laws.Africa Content API](../../api/about-the-api.md). The legislation is split into portions (chapters, sections, paragraphs, etc.) and embeddings are calculated for each portion. These embeddings make it possible to perform semantic queries against the data to find portions relevant to a keyword, phrase or question.

{% hint style="info" %}
Knowledge Bases include only the most recent version of legislation. See [Works and Expressions](../../get-started/works-and-expressions.md) for more details on how legislation changes over time.
{% endhint %}

### Judgments Knowledge Bases

Our judgments Knowledge Bases contain the full text of all the court judgments (case law) that is available from our various Legal Information Institute (LII) partner websites. The judgments are split into chunks of text (along page boundaries, if pages are available) and embeddings are calculated for each chunk. These embeddings make it possible to perform semantic queries against the judgment dataset.

## Legislation chunking and embedding process

Our legislation chunking and embedding process is based on the hierarchical structure of the legislation document. This makes it possible to find specific sections of legislation that match a query.

1. The text for all chapters, parts and sections are extracted. We do not process individual provisions below section level, such as paragraphs or sub-paragraphs.
2. The headings of the containing chapters and parts are added to each provision's text for additional context.
3. The text for each provision is chunked at about 256 tokens, split along sentence boundaries with a small overlap.
4. Embeddings are calculated for each chunk. Currently we use [Cohere's embed-multilingual-v3](https://docs.cohere.com/docs/cohere-embed).

