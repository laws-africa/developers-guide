# Types of Knowledge Bases

Laws.Africa has two types of Knowledge Bases:

1. Legislation Knowledge Bases that contain legislation documents (Acts, by-laws, regulations)
2. Judgments Knowledge Bases that contain court judgments

## Legislation Knowledge Bases

Our legislation Knowledge Bases contain the full text of all the digitised legislation that is available through the [Laws.Africa Content API](../../api/about-the-api.md). The legislation is split into portions (chapters, sections, paragraphs, etc.) and embeddings are calculated for each portion. These embeddings make it possible to perform semantic queries against the data to find portions relevant to a keyword, phrase or question.

{% hint style="info" %}
Knowledge Bases include only the most recent version of legislation. See [Works and Expressions](../../get-started/works-and-expressions.md) for more details on how legislation changes over time.
{% endhint %}

### Querying a legislation Knowledge Base

Query a legislation Knowledge Base using the [retrieve API](query-a-knowledge-base.md).

The API will return **portions** of legislation that match your query, such as chapters, sections or schedules, including metadata and the full text of the portion. You can use these portions to respond to a user's query or to display to the user.

In some cases, the portion may be a page of a PDF. This happens when Laws.Africa has not (yet) converted that legislation into Akoma Ntoso format, and so a clean textual version of the legislation is not available.

{% hint style="success" %}
You usually want to apply filters to legislation queries to restrict results to [principal](../../get-started/works-and-expressions.md), un-repealed legislation.

```
filters: {"principal": true, "repealed": false}
```
{% endhint %}

For more details, see [using-a-knowledge-base.md](using-a-knowledge-base.md "mention")

### Legislation chunking and embedding

Our legislation chunking and embedding process is based on the hierarchical structure of the legislation document. This makes it possible to find specific sections of legislation that match a query.

1. The text for all chapters, parts and sections are extracted. We do not process individual provisions below section level, such as paragraphs or sub-paragraphs.
2. The headings of the containing chapters and parts are added to each provision's text for additional context.
3. The text for each provision is chunked at about 256 tokens, split along sentence boundaries with a small overlap.
4. Embeddings are calculated for each chunk. Currently we use [Cohere's embed-multilingual-v3](https://docs.cohere.com/docs/cohere-embed).

## Judgments Knowledge Bases

Our judgments Knowledge Bases contain the full text and summaries of the court judgments (case law) that is available from our various Legal Information Institute (LII) partner websites. The judgments are split into chunks of text (along page boundaries, if pages are available) and embeddings are calculated for each chunk. Embeddings are also calculated separately for the judgment summaries. These embeddings make it possible to perform semantic queries against the judgment dataset.

### Querying a judgment Knowledge Base

Query a legislation Knowledge Base using the [retrieve API](query-a-knowledge-base.md).

The API will return judgment metadata and textual summaries of matching judgments. These summaries can be used to respond to a user's query or displayed to the user.

{% hint style="warning" %}
The API does not return the original text of the judgment, only the summary.
{% endhint %}

For more details, see [using-a-knowledge-base.md](using-a-knowledge-base.md "mention")

### Judgment chunking and embedding

Our judgment chunking and embedding process is applied to the full text and summary of the judgment.

1. The text for the entire judgment is extracted per-page for PDF files, and as one large string for HTML files.
2. The summary fields (blurb, flynote, summary, issues, holdings and order) are extracted as a separate string.
3. The text for each string is chunked at about 256 tokens, split along sentence boundaries with a small overlap.
4. Embeddings are calculated for each chunk. Currently we use [Cohere's embed-multilingual-v3](https://docs.cohere.com/docs/cohere-embed).
