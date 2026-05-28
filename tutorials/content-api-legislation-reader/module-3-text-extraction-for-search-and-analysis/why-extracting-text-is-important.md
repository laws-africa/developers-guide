---
description: Why would you want to extract text from an Akoma Ntoso XML document?
---

# Why extracting text is important

Akoma Ntoso XML documents and the resulting HTML documents contain rich, structured information. While this structure is important for displaying, formatting and analysis of the document, sometimes it's important to work with just the text content.

Extracting text content from a structure Akoma Ntoso XML or HTML document is useful for use cases such as:

* Indexing into Elasticsearch or other search engines to support full-text search
* Calculating [embeddings](https://en.wikipedia.org/wiki/Word\_embedding) for use with machine learning models such as Large-Language Models (LLMs)
* Natural-language processing and analysis (NLP)

The text content we need depends on our use case. We may need to ignore certain text content such as metadata, editorial comments, embedded text, headings and portion numbers.

To extract text content from a document, we process the XML and use XML-based queries to extract just the text we're interested in.

In this module we'll explore how to extract text content for different uses cases.
