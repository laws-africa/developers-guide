---
description: Some key details before we build our legislation app.
---

# Introductory concepts

### In this section

* Works and Expressions
* FRBR URIs

### Works and expressions

A **work** is a piece of legislation.

It _does_ include all the metadata concerning publication and other lifecycle events (commencements, amendments, and repeals).

It _does not_ include the text and other content, because there can be more than one version of the content (depending on the language and date).

An **expression** is the content of a work, in a given language and at a given date.

A work can have multiple expressions. Most of the metadata on an expression is inherited from its work.

One key difference between a work and an expression is that an expression can have a different title from the work's, according to its language and date.

The language and date belong to the expression only.

#### FRBR URI essentials

An FRBR URI uniquely identifies a work and an expression.

The FRBR URI is created using some core metadata of the work and expression.

Work-level FRBR URI example:&#x20;

> `/akn/za/act/1996/93`

Because an expression inherits these core pieces of metadata from its work, an expression-level FRBR URI is the parent URI, plus the language and the date of the expression.

Expression-level FRBR URI examples:&#x20;

> `/akn/za/act/1996/93/eng@1996-11-22`
>
> `/akn/za/act/1996/93/eng@2000-08-01`

In the above example, the expressions are the English-language versions, from 22 November 1996 onwards, and from 1 August 2000 onwards, of a South African national Act, 93 of 1996. The work is Act 93 of 1996, which can and does have multiple expressions.

While the work's title is the National Road Traffic Act, the expressions may have different titles. This is more common in the case of different language versions, although some pieces of legislation are renamed over time. In those cases, the older expressions will have the old title, and the work and the newer expressions will have the new one.
