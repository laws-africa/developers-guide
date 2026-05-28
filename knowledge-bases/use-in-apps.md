---
description: Use Knowledge Base results in RAG, search and legal agent workflows.
---

# Use results in your app

Knowledge Base results are designed to be passed into product workflows as legal context. They work well in RAG systems,
legal search interfaces and agent tool calls.

## Legal search

For a search interface:

1. send the user's search text to the retrieve endpoint;
2. show result titles, excerpts and source URLs;
3. group legislation results by `work_frbr_uri` when several portions come from the same work;
4. let users open `portion_public_url` or `public_url` to inspect the source.

## RAG and AI answers

For a RAG workflow:

1. retrieve the most relevant results;
2. build a context block from `content.text`, `title`, `portion_title` and source URLs;
3. ask the model to answer only from the provided context;
4. include citations or source links in the final answer.

Example context format:

```text
Source: Animal By-law, 2011
URL: https://lawlibrary.org.za/akn/za-cpt/act/by-law/2011/animal/eng@2011-08-05#chp_7
Portion: Chapter 7 - Miscellaneous
Text: ...
```

{% hint style="warning" %}
Knowledge Base retrieval provides legal context. It does not replace legal
review, and AI-generated answers should make their sources clear.
{% endhint %}

## Agent tools

For a legal agent, expose retrieval as a tool with inputs such as:

* `query`: focused legal search text;
* `knowledge_base`: the KB code to query;
* `place`: optional place filter;
* `top_k`: result count.

The agent should use the returned legal text and source URLs before drafting an answer or deciding whether it needs more
information.

## Improve retrieval quality

If results are too broad:

* add place filters such as `frbr_place`;
* apply `principal: true` and `repealed: false` for legislation;
* reduce `top_k`;
* rewrite the user question into more specific legal search terms.

If results are too narrow:

* remove filters;
* increase `top_k`;
* query a broader Knowledge Base;
* try alternative legal terms.

## More examples

See the example repository for runnable integrations:

[https://github.com/laws-africa/knowledge-base-examples](https://github.com/laws-africa/knowledge-base-examples)
