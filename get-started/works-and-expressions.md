---
description: Understand these two important concepts.
---

# Works and expressions

<figure><img src="../.gitbook/assets/image.avif" alt=""><figcaption></figcaption></figure>

Two important concepts that are an essential part of the API are **works** and **expressions**.

* A **Work** is a piece of legislation, such as an act, regulation or by-law. A work may be amended over time and may even have its title changed. A work is uniquely identified by a _work FRBR URI_ which never changes.
* An **Expression** is a version of a Work in specific language at a particular point in time. A work can have many expressions, usually one for each official language and amendment. An expression is uniquely identified by its own _expression FRBR URI_, which is derived from the work's FRBR URI.

An example of a work is the South African _Employment Equity Amendment Act, 2013 (Act 55 of 1998)_ with unique work FRBR URI `/akn/za/act/1998/55`. This act has been amended a number of times since it was first passed. Each amended version (also called a _point in time_) is a unique expression of the work.

The English expression of the work, as it was amended on 17 January 2014, is uniquely identified by the expression FRBR URI `/akn/za/act/1998/55/eng@2014-01-17`. You can see that this is built from the work's URI, with a language code `eng` and the expression date `2014-01-17` included.

{% hint style="info" %}
When fetching details from the API, you are always fetching details for a particular expression of the work. The expression will also include information related to the expression's work, such as the work's FRBR URI and publication information. Even if you don't specific a particular date for the expression, the API will return the latest expression applicable at the time of the request.
{% endhint %}

{% hint style="info" %}
Read more about the terminology used by Laws.Africa in our [Terminology Guide](https://docs.laws.africa/getting-started/terminology-guide).
{% endhint %}

## Akoma Ntoso FRBR URIs

The API relies heavily on Akoma Ntoso FRBR URIs, which are described in the [Akoma Ntoso naming convention standard](http://docs.oasis-open.org/legaldocml/akn-nc/v1.0/akn-nc-v1.0.html).

When we use a URL such as `/v3/frbr-uri/` in this guide, the `frbr-uri` part is a full FRBR URI, such as `/akn/za/act/1998/84/eng`.
