---
title: Content API Documentation
position: 1
lead: >-
  Use the Laws.Africa Content API to use legislation in your website, app or
  research.
redirect_from: /api/docs
description: >-
  Use the Laws.Africa Content API to use legislation in your website, app or
  research.
---

# Content API Reference

This guide is for developers who want to use the API to fetch legislative works. We assume that you have a basic understanding of REST APIs and the [Akoma Ntoso](http://www.akomantoso.org/) standard for legislation \(acts in particular\).

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

## Quick Start

First, sign up for a free Laws.Africa account at [https://edit.laws.africa/accounts/login/](https://edit.laws.africa/accounts/login/).

Next, get your API token from [https://edit.laws.africa/accounts/profile/api/](https://edit.laws.africa/accounts/profile/api/).

{% hint style="info" %}
In the examples below, replace `<YOUR_AUTH_TOKEN>` with your personal API token.
{% endhint %}

Let's fetch the details of Cape Town's Animal by-law, in JSON format. This includes the title, publication details and a list of other API calls you can make for additional details.

```bash
$ curl -H "Authorization: Token <YOUR_AUTH_TOKEN>" \
  https://api.laws.africa/v2/akn/za-cpt/act/by-law/2011/animal.json
```

Laws.Africa can provide us with a Table of Contents for the by-law, also in JSON format. Let's fetch that:

```bash
$ curl -H "Authorization: Token <YOUR_AUTH_TOKEN>" \
  https://api.laws.africa/v2/akn/za-cpt/act/by-law/2011/animal/toc.json
```

Now let's get the HTML content of Section 3 of the by-law, regarding dog registration and licensing:

```bash
$ curl -H "Authorization: Token <YOUR_AUTH_TOKEN>" \
  https://api.laws.africa/v2/akn/za-cpt/act/by-law/2011/animal/eng/main/section/3.html
```

Finally, let's put that HTML into a webpage and include some stylesheets to make it look good:

```markup
<link rel="stylesheet" type="text/css"
      href="https://cdn.jsdelivr.net/gh/laws-africa/indigo-web/css/akoma-ntoso.min.css">

<div class="akoma-ntoso">
  <section class="akn-section" id="section-3" data-id="section-3">
    <h3>3. Dog registration and licensing</h3>
    <section class="akn-subsection" id="section-3.1" data-id="section-3.1">
      <span class="akn-num">(1)</span>
      <span class="akn-content"><span class="akn-p">The <span class="akn-term" data-refersTo="#term-owner" id="trm78" data-id="trm78">owner</span> of a property where one or more dogs are kept must register the <span class="akn-term" data-refersTo="#term-dog" id="trm79" data-id="trm79">dog</span> or dogs with the <span class="akn-term" data-refersTo="#term-Council" id="trm80" data-id="trm80">Council</span>.</span></span>
    </section>
    <section class="akn-subsection" id="section-3.2" data-id="section-3.2">
      <span class="akn-num">(2)</span>
      <span class="akn-content"><span class="akn-p">Dog registration must take place within four months of the <span class="akn-term" data-refersTo="#term-dog" id="trm81" data-id="trm81">dog</span>&#8217;s birth or within 30 days of acquiring a <span class="akn-term" data-refersTo="#term-dog" id="trm82" data-id="trm82">dog</span> on property within <span class="akn-term" data-refersTo="#term-Council" id="trm83" data-id="trm83">Council</span>&#8217;s jurisdictional boundaries.</span></span>
    </section>
    <section class="akn-subsection" id="section-3.3" data-id="section-3.3">
      <span class="akn-num">(3)</span>
      <span class="akn-content"><span class="akn-p">The <span class="akn-term" data-refersTo="#term-Council" id="trm84" data-id="trm84">Council</span> may levy a <span class="akn-term" data-refersTo="#term-dog" id="trm85" data-id="trm85">dog</span> license fee in respect of a property where one or more dogs are kept.</span></span>
    </section>
    <section class="akn-subsection" id="section-3.4" data-id="section-3.4">
      <span class="akn-num">(4)</span>
      <span class="akn-content"><span class="akn-p">The amount of the <span class="akn-term" data-refersTo="#term-dog" id="trm86" data-id="trm86">dog</span> license fee may be determined in terms of a resolution of <span class="akn-term" data-refersTo="#term-Council" id="trm87" data-id="trm87">Council</span>. A reduced <span class="akn-term" data-refersTo="#term-dog" id="trm88" data-id="trm88">dog</span> license fee may apply for sterilized dogs.</span></span>
    </section>
  </section>
</div>
```

![](../.gitbook/assets/section-3-html.png)

## Works and Expressions

Two important concepts that are an essential part of the API are **works** and **expressions**.

* A **Work** is a piece of legislation, such as an act, regulation or by-law. A work may be amended over time and may even have its title changed. A work is uniquely identified by a _work FRBR URI_ which never changes.
* An **Expression** is a version of a Work in specific language at a particular point in time. A work can have many expressions, usually one for each official language and amendment. An expression is uniquely identified by its own _expression FRBR URI_, which is derived from the work's FRBR URI.

An example of a work is the South African _Employment Equity Amendment Act, 2013 \(Act 55 of 1998\)_ with unique work FRBR URI `/akn/za/act/1998/55`. This act has been amended a number of times since it was first passed. Each amended version \(also called a _point in time_\) is a unique expression of the work.

The English expression of the work, as it was amended on 17 January 2014, is uniquely identified by the expression FRBR URI `/act/1998/55/eng@2014-01-17`. You can see that this is built from the work's URI, with a language code `eng` and the expression date `2014-01-17` included.

{% hint style="info" %}
When fetching details from the API, you are always fetching details for a particular expression of the work. The expression will also include information related to the expression's work, such as the work's FRBR URI and publication information. Even if you don't specific a particular date for the expression, the API will return the latest expression applicable for the date of the request.
{% endhint %}

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

Not all responses support all formats, the documentation will be explicit about what is supported. {.alert.alert-info}

## Countries and Localities

```text
GET https://api.laws.africa/v2/countries.json
```

This returns a list of the countries and localities that Laws.Africa knows about. It includes links to the APIs for listing works for each country and locality.

## Listing Works

```text
GET https://api.laws.africa/v2/akn/za/
GET https://api.laws.africa/v2/akn/za/act/
GET https://api.laws.africa/v2/akn/za/act/2007/
```

* Content types: JSON, PDF, EPUB, ZIP

These endpoints list the works for a country or year. To list the available works for a country you'll need the [two-letter country code](http://en.wikipedia.org/wiki/ISO_3166-1_alpha-2) for the country.

The listings include the most recent applicable expressions of each work, in the country's default language.

## Fetching a Work

```text
GET https://api.laws.africa/v2/frbr-uri.json
```

This returns the detail of an expression of a work as a JSON document. For example, this is the description of the English expression of Act 55 of 1998 as at 2014-01-07.

```javascript
{
  "url": "https://api.laws.africa/v2/akn/za/act/1998/55/eng.json",
  "title": "Employment Equity Act, 1998",
  "created_at": "2017-12-23T10:05:55.105543Z",
  "updated_at": "2018-06-07T08:07:51.170250Z",
  "country": "za",
  "locality": null,
  "nature": "act",
  "subtype": null,
  "year": "1998",
  "number": "55",
  "frbr_uri": "/akn/za/act/1998/55",
  "expression_frbr_uri": "/akn/za/act/1998/55/eng@2005-10-03",
  "publication_date": "1998-10-19",
  "publication_name": "Government Gazette",
  "publication_number": "19370",
  "expression_date": "2014-01-17",
  "commencement_date": "1999-05-14",
  "assent_date": "1998-10-12",
  "language": "eng",
  "repeal": null,
  "amendments": [
    {
      "date": "2014-01-17",
      "amending_title": "Employment Equity Amendment Act, 2013",
      "amending_uri": "/akn/za/act/2013/47"
    },
  ],
  "points_in_time": [
    {
      "date": "2014-01-17",
      "expressions": [
        {
          "url": "https://api.laws.africa/v2/akn/za/act/1998/55/eng@2014-01-17",
          "language": "eng",
          "expression_frbr_uri": "/akn/za/act/1998/55/eng@2014-01-17",
          "expression_date": "2014-01-17",
          "title": "Employment Equity Act, 1998"
        }
      ]
    }
  ],
  "links": [
    {
      "href": "https://api.laws.africa/v2/act/akn/za/act/1998/55/eng@2014-01-17.html",
      "title": "HTML",
      "rel": "alternate",
      "mediaType": "text/html"
    },
    { "..." }
  ]
}
```

The fields of the response are described in the table below.

| Field | Description | Type |
| :--- | :--- | :--- |
| amendments | List of amendments that have been applied to create this expression of the work. | See below |
| assent\_date | Date when the work was assented to. | ISO8601 |
| content\_url | URL of the full content of the work. | URL |
| country | ISO 3166-1 alpha-2 country code that this work is applicable to. | String |
| created\_at | Timestamp of when the work was first created. | ISO8601 |
| draft | Is this a draft work or is it available in the public API? | Boolean |
| expression\_date | Date of this expression of the work. | ISO8601 |
| commencement\_date | Date on which this work commences. | ISO8601 |
| expression\_frbr\_uri | FRBR URI of this expression of this work. | String |
| frbr\_uri | FRBR URI for this work. | String |
| id | Unique ID of this work. | Integer |
| language | Three letter ISO-639-2 language code for this expression of the work. | String |
| links | A description of links to other formats of this expression that are available through the API. | Array |
| locality | The code of the locality within the country. | String |
| nature | The nature of this work, normally "act". | String |
| number | Number of this work with its year, or some other unique way of identifying it within the year. | String |
| publication\_date | Date of original publication of the work. | ISO8601 |
| publication\_name | Name of the publication in which the work was originally published. | String |
| publication\_number | Number of the publication in which the work was originally published. | String |
| repeal | Description of the repeal of this work, if it has been repealed. | See below |
| subtype | Subtype code of the work. | String |
| title | Short title of the work, in the appropriate language. | String |
| updated\_at | Timestamp of when the work was last updated. | ISO8601 |
| url | URL for fetching details of this work. | URL |
| year | Year of the work. | Stri |

#### Amendments

The fields of the `amendments` property of the response are described below.

| Field | Description | Type |
| :--- | :--- | :--- |
| amending\_title | Title of the amending work | String |
| amending\_uri | Work FRBR URI of the amending work | String |
| date | Date on which the amendment takes place | ISO8601 |

#### Points in Time

The fields of the `points_in_time` property of the response are described below.

| Field | Description | Type |
| :--- | :--- | :--- |
| date | Date of the point-in-time for which expressions are available | ISO8601 |
| expressions | A list of expressions for this work available at this point in time |  |
| url | The API URL to fetch information on the expression | URL |
| language | Three-letter language code of the language of the expression | String |
| expression\_frbr\_uri | Unique Expression FRBR URI for this expression | String |
| expression\_date | Date of this expression | ISO8601 |
| title | Title of the work, appropriate for the expression in the expression's language\) | String |

### Akoma Ntoso

```text
GET https://api.laws.africa/v2/frbr-uri.xml
```

This returns the Akoma Ntoso XML of an expression of a work.

For example, fetch the most recent applicable English Akoma Ntoso expression of `/akn/za/act/1998/55` by calling:

```text
GET https://api.laws.africa/v2/akn/za/act/1998/55/eng.xml
```

### HTML

```text
GET https://api.laws.africa/v2/frbr-uri.html
```

Fetch the HTML version of a work by specify `.html` as the format extensions in the URL.

* Parameter `coverpage`: should the response contain a generated coverpage? Use 1 for true, anything else for false. Default: 1.
* Parameter `standalone`: should the response by a full HTML document, including CSS, that can stand on its own? Use 1 for true, anything else for false. Default: false.
* Parameter `resolver`: the fully-qualified URL to use when resolving absolute references to other Akoma Ntoso documents. Use 'no' or 'none' to disable. Default is to use the Laws.Africa resolver.
* Parameter `media-url`: the fully-qualified URL prefix to use when generating links to media, such as images.

For example, fetch the most recent applicable English HTML expression of `/akn/za/act/1998/55` by calling:

```text
GET https://api.laws.africa/v2/akn/za/act/1998/55/eng.html
```

### Expressions

You can fetch specific expressions of a work by including expression-specific information in the requested FRBR URI. The API supports the language and date aspects defined in the [Akoma Ntoso naming convention standard](http://docs.oasis-open.org/legaldocml/akn-nc/v1.0/akn-nc-v1.0.html).

For example, this request will fetch the HTML of the English expression of Act 55 of 1998, as amended on 2014-01-17:

```text
GET https://api.laws.africa/v2/akn/za/act/1998/55/eng@2014-01-17.html
```

The available expressions of a work are listed in the `points_in_time` field of the JSON description of the work. Each point in time includes a date and a list of expressions available at that date, one for each available language.

You can use the following date formats to request different expressions of a work.

| Date Format | Meaning | Example Expression FRBR URI |
| :--- | :--- | :--- |
| `@` | Very first expression of a work. | `/akn/za/act/1998/55/eng@` |
| &lt;code&gt;@YYYY-MM-DD&lt;/code&gt; | Expression at the specific date. | `/akn/za/act/1998/55/eng@2014-01-17` |
| `:YYYY-MM-DD` | Most recent expression at or before a date. | `/akn/za/act/1998/55/eng:2015-01-01` |
| \(none\) | The most recent expression at or before today's date. Equivalent to using `:` with today's date. | `/akn/za/act/1998/55/eng` |

The `.format` part of the FRBR URI is placed after the `@YYYY-MM-DD` part.

{% hint style="info" %}
If you use `@` to specify a particular date and the API doesn't have a version at exactly that date, it will return a 404 response. If you need the expression of the work closest to a particular date, use `:` instead. 
{% endhint %}

### Table of Contents

```text
GET https://api.laws.africa/v2/frbr-uri/toc.json
```

* Content types: JSON

Get a description of the table of contents \(TOC\) of an act. This includes the chapters, parts, sections and schedules that make up the act, based on the structure captured by the Laws.Africa editor.

Each item in the table of contents has this structure:

```javascript
{
  "id": "chapter-1",
  "type": "chapter",
  "num": "1",
  "heading": "Interpretation",
  "title": "Chapter 1 - Interpretation",
  "component": "main",
  "subcomponent": "chapter/1",
  "url": "http://api.laws.africa/v2/akn/za/act/1998/10/eng/main/chapter/1",
  "children": [ "..." ]
}
```

Each of these fields is described in the table below.

| Field | Description | Type |
| :--- | :--- | :--- |
| id | The unique XML element id of this item. \(optional\) | String |
| type | The Akoma Ntoso element name of this item. | String |
| num | The number of this item, such as a chapter, part or section number. \(optional\) | String |
| heading | The heading of this item \(optional\) | String |
| title | A derived, friendly title of this item, taking `num` and `heading` into account and providing good defaults if either of those is missing. | String |
| component | The component of the Akoma Ntoso document that this item is a part of, such as `main` for the main document, or `schedule1` for the first schedule. | String |
| subcomponent | The subcomponent of the component that this item is a part of, such as a chapter. \(optional\) | String |
| url | The API URL for this item, which can be used to fetch XML, HTML and other details of just this part of the document. | String |
| children | A possibly-empty array of TOC items that are children of this item. | Array |

### Parts, Chapters and Sections

You can use the `url` field from an item in the table of contents to fetch the details of just that item in various forms.

```text
GET https://api.laws.africa/v2/frbr-uri/toc-item-uri.format
```

* Content types: XML, HTML, PDF, ePUB, ZIP

## Using HTML Responses

Laws.Africa transforms Akoma Ntoso XML into HTML5 content that looks best when styled with [Indigo Web](https://github.com/laws-africa/indigo-web) stylesheets. You can link to the stylesheets provided by that package, or you can pull them into your website.

## Search

```text
GET https://api.laws.africa/v2/search/<country>?q=<search-term>
```

* Where `<country>` is a two-letter country code
* Parameter `q`: the search string
* Content types: JSON

This API searches for works in a country. It returns all works that match the search term in either their title or their body. Results are returned in search rank order. Each result also has a numeric `_rank` and an HTML `_snippet` with highlighted results.

If more than one expression of a particular work matches the search, then only the most recent matching expression is returned.
