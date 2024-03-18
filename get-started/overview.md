---
title: Content API Documentation
position: 1
lead: >-
  Use the Laws.Africa Content API to use legislation in your website, app or
  research.
redirect_from: /api/docs
---

# Introduction

This guide is for developers who want to use the Laws.Africa Content API to fetch legislative content and metadata from Laws.Africa. We assume that you have a basic understanding of REST APIs and the [Akoma Ntoso](http://www.akomantoso.org/) standard for legislation (acts in particular).

The API is a read-only API for listing and fetching published versions of legislative works. Using it, you can:

* get a list of places (countries and localities)
* get a list of all works by country and year
* get a JSON description of a work
* get Akoma Ntoso XML for a work
* get a human-friendly HTML version of a work

## Key concepts

### Places

All legislation content is contained in a **place**, which is either a **country** or a **locality** inside a country.

A **country** is identified by a unique [ISO3166 two-letter country code](https://en.wikipedia.org/wiki/ISO\_3166-1\_alpha-2) such as `za` for South Africa.

A **locality** is a place inside a country such as a province, county, state or municipality. A locality is identified by a unique code, which is made up of the country code, a hypen, and a unique code assigned to the locality by a suitable authority. For example, the code for Cape Town, South Africa is `za-cpt`.

### FRBR URIs

**FRBR URIs** are used heavily in the Laws.Africa API to identify works and expressions.

The FRBR URI is part of the [Akoma Ntoso Naming Convention](https://docs.oasis-open.org/legaldocml/akn-nc/v1.0/akn-nc-v1.0.html) standard and builds on the principles in the [Functional Requirements for Bibliographic Records (FRBR)](https://en.wikipedia.org/wiki/Functional\_Requirements\_for\_Bibliographic\_Records).

An example FRBR URI is `/akn/za/act/1998/55` which identifies South African Act 55 of 1998. It is made up of these parts:

1. `akn` - this prefix indicates the URI uses the Akoma Ntoso (AKN) format
2. `za` - the place code identifying South Africa
3. `act` - the work type
4. `1998` - the year of the act
5. `55` - the number of the act

### Works and expressions

Two important concepts that are an essential part of the API are **works** and **expressions**.

* A **Work** is a piece of legislation, such as an act, regulation or by-law. A work may be amended over time and may even have its title changed. A work is uniquely identified by a _work FRBR URI_ which never changes.
* An **Expression** is a version of a Work in specific language at a particular point in time. A work can have many expressions, usually one for each official language and amendment. An expression is uniquely identified by its own _expression FRBR URI_, which is derived from the work's FRBR URI.

An example of a work is the South African _Employment Equity Amendment Act, 2013 (Act 55 of 1998)_ with unique work FRBR URI `/akn/za/act/1998/55`. This act has been amended a number of times since it was first passed. Each amended version (also called a _point in time_) is a unique expression of the work.

The English expression of the work, as it was amended on 17 January 2014, is uniquely identified by the expression FRBR URI `/act/1998/55/eng@2014-01-17`. You can see that this is built from the work's URI, with a language code `eng` and the expression date `2014-01-17` included.

## Formatting Legislation HTML with Law Widgets

Laws.Africa transforms Akoma Ntoso XML into HTML content that looks best when styled with [Laws.Africa's Law Widgets](https://github.com/laws-africa/law-widgets).
