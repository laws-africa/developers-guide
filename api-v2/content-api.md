---
title: Content API Documentation
position: 1
lead: >-
  Use the Laws.Africa Content API to use legislation in your website, app or
  research.
redirect_from: /api/docs
description: >-
  This describes the Laws.Africa Content API. The latest version of the API is
  version 2.
---

# Overview

This guide is for developers who want to use the Laws.Africa Content API to fetch legislative content and metadata from Laws.Africa. We assume that you have a basic understanding of REST APIs and the [Akoma Ntoso](http://www.akomantoso.org/) standard for legislation \(acts in particular\).

{% hint style="info" %}
The API relies heavily on Akoma Ntoso FRBR URIs, which are described in the [Akoma Ntoso naming convention standard](http://docs.oasis-open.org/legaldocml/akn-nc/v1.0/akn-nc-v1.0.html).
{% endhint %}

The API is a read-only API for listing and fetching published versions of legislative works. Using it, you can:

* get a list of countries and localities
* get a list of all works by country and year
* get a JSON description of a work
* get Akoma Ntoso XML for a work
* get a human-friendly HTML version of a work

{% hint style="info" %}
When we use a URL such as `/v2/frbr-uri/` in this guide, the `frbr-uri` part is a full FRBR URI, such as `/akn/za/act/1998/84/eng`.
{% endhint %}

## Endpoint

The latest version of the API is version 2. It is available at the endpoint:

```javascript
https://api.laws.africa/v2/
```

## Authentication

Calls to the API must be authenticated using your API token. To get a token,

1. Sign up for a free Laws.Africa account at [https://edit.laws.africa/accounts/login/](https://edit.laws.africa/accounts/login/).
2. Get your API token from [https://edit.laws.africa/accounts/profile/api/](https://edit.laws.africa/accounts/profile/api/).

Include your API token in your API calls by including an `Authorization` header:

```text
Authorization: Token YOUR_AUTH_TOKEN
```

If you're logged into your Laws.Africa account, you can also browse the API using your web browser and visiting [https://api.laws.africa/v2/countries](https://api.laws.africa/v2/countries).

## Pagination

API calls that return lists will be paginated and return a limited number of items per page. The response includes information on the total number of items and the URLs to use to fetch the next and previous pages of items.

Here's an example of the first page of a paginated response with 250 total items and two pages:

```javascript
{
  "count": 250,
  "next": "https://api.laws.africa/v2/akn/za.json?page=2",
  "previous": null,
  "results": [ "..." ]
}
```

In this case, fetching the `next` URL will return the second \(and final\) page.

## Content types

Some API calls can return content in multiple formats. You can specify the required content of your request by placing `.format` at the end of the URL. In most cases the default response type is JSON.

* `.json` or `Accept: application/json`: return JSON
* `.xml` or `Accept: application/xml`: return Akoma Ntoso XML
* `.html` or `Accept: text/html`: return human friendly HTML
* `.epub` or `Accept: application/epub+zip`: return an ePUB \(ebook\) document
* `.pdf` or `Accept: application/pdf`: return a PDF document
* `.zip` or `Accept: application/zip`: return a ZIP file with the document XML and media attachments

{% hint style="info" %}
Not all responses support all formats, the documentation will be explicit about what is supported.
{% endhint %}

## Using HTML Responses

Laws.Africa transforms Akoma Ntoso XML into HTML5 content that looks best when styled with [Indigo Web](https://github.com/laws-africa/indigo-web) stylesheets. You can link to the stylesheets provided by that package, or you can pull them into your website.

