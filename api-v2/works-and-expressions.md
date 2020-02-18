---
description: Listing and fetching works and expressions.
---

# Works and expressions

Two important concepts that are an essential part of the API are **works** and **expressions**.

* A **Work** is a piece of legislation, such as an act, regulation or by-law. A work may be amended over time and may even have its title changed. A work is uniquely identified by a _work FRBR URI_ which never changes.
* An **Expression** is a version of a Work in specific language at a particular point in time. A work can have many expressions, usually one for each official language and amendment. An expression is uniquely identified by its own _expression FRBR URI_, which is derived from the work's FRBR URI.

An example of a work is the South African _Employment Equity Amendment Act, 2013 \(Act 55 of 1998\)_ with unique work FRBR URI `/akn/za/act/1998/55`. This act has been amended a number of times since it was first passed. Each amended version \(also called a _point in time_\) is a unique expression of the work.

The English expression of the work, as it was amended on 17 January 2014, is uniquely identified by the expression FRBR URI `/act/1998/55/eng@2014-01-17`. You can see that this is built from the work's URI, with a language code `eng` and the expression date `2014-01-17` included.

{% hint style="info" %}
When fetching details from the API, you are always fetching details for a particular expression of the work. The expression will also include information related to the expression's work, such as the work's FRBR URI and publication information. Even if you don't specific a particular date for the expression, the API will return the latest expression applicable for the date of the request.
{% endhint %}

{% hint style="info" %}
The API supports the language and date aspects defined in the [Akoma Ntoso naming convention standard](http://docs.oasis-open.org/legaldocml/akn-nc/v1.0/akn-nc-v1.0.html).
{% endhint %}

{% api-method method="get" host="https://api.laws.africa" path="/v2/akn/:country/.:format" %}
{% api-method-summary %}
List works for a place
{% endapi-method-summary %}

{% api-method-description %}
This endpoint lists works for a country or locality.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="country" type="string" required=true %}
Country or country-locality code.
{% endapi-method-parameter %}

{% api-method-parameter name="format" type="string" required=false %}
Response format.
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}

{% api-method-headers %}
{% api-method-parameter name="Authentication" type="string" required=true %}
Authentication token to identify you.
{% endapi-method-parameter %}
{% endapi-method-headers %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}
Paginated list of works in JSON format.
{% endapi-method-response-example-description %}

```javascript
{
  "count": 1,
  "next": null,
  "previous": null,
  "results": [
    {
      "url": "https://api.laws.africa/v2/akn/za-wc033/act/by-law/2005/beaches/eng@2005-10-03",
      "title": "By-Law relating to Beaches",
      "created_at": "2019-06-27T18:28:27.231180Z",
      "updated_at": "2019-06-27T18:28:27.481534Z",
      "country": "za",
      "locality": "wc033",
      "nature": "act",
      "subtype": "by-law",
      "year": "2005",
      "number": "beaches",
      "frbr_uri": "/akn/za-wc033/act/by-law/2005/beaches",
      "expression_frbr_uri": "/akn/za-wc033/act/by-law/2005/beaches/eng@2005-10-03",
      "publication_date": "2005-10-03",
      "publication_name": "Western Cape Provincial Gazette",
      "publication_number": "6303",
      "publication_document": {
        "url": "https://archive.opengazettes.org.za/archive/ZA-WC/2005/provincial-gazette-ZA-WC-no-6303-dated-2005-10-03.pdf",
        "filename": "za-wc033-act-by-law-2005-beaches-publication-document.pdf",
        "mime_type": "",
        "size": null
      },
      "commenced": false,
      "commencement_date": null,
      "commencing_work": null,
      "assent_date": null,
      "repeal": null,
      "parent_work": null,
      "expression_date": "2005-10-03",
      "language": "eng",
      "points_in_time": [
        {
          "date": "2005-10-03",
          "expressions": [
            {
              "title": "By-Law relating to Beaches",
              "language": "eng",
              "expression_date": "2005-10-03",
              "expression_frbr_uri": "/akn/za-wc033/act/by-law/2005/beaches/eng@2005-10-03",
              "url": "https://api.laws.africa/v2/akn/za-wc033/act/by-law/2005/beaches/eng@2005-10-03"
            },
            {
              "title": "Verordening insake Strande",
              "language": "afr",
              "expression_date": "2005-10-03",
              "expression_frbr_uri": "/akn/za-wc033/act/by-law/2005/beaches/afr@2005-10-03",
              "url": "https://api.laws.africa/v2/akn/za-wc033/act/by-law/2005/beaches/afr@2005-10-03"
            }
          ]
        }
      ],
      "amendments": [],
      "stub": false,
      "numbered_title": null,
      "taxonomies": null,
      "as_at_date": "2005-10-03",
      "links": [
        {
          "rel": "alternate",
          "title": "HTML",
          "href": "https://api.laws.africa/v2/akn/za-wc033/act/by-law/2005/beaches/eng@2005-10-03.html",
          "mediaType": "text/html"
        },
        {
          "rel": "alternate",
          "title": "Standalone HTML",
          "href": "https://api.laws.africa/v2/akn/za-wc033/act/by-law/2005/beaches/eng@2005-10-03.html?standalone=1",
          "mediaType": "text/html"
        },
        {
          "rel": "alternate",
          "title": "Akoma Ntoso",
          "href": "https://api.laws.africa/v2/akn/za-wc033/act/by-law/2005/beaches/eng@2005-10-03.xml",
          "mediaType": "application/xml"
        },
        {
          "rel": "alternate",
          "title": "PDF",
          "href": "https://api.laws.africa/v2/akn/za-wc033/act/by-law/2005/beaches/eng@2005-10-03.pdf",
          "mediaType": "application/pdf"
        },
        {
          "rel": "alternate",
          "title": "ePUB",
          "href": "https://api.laws.africa/v2/akn/za-wc033/act/by-law/2005/beaches/eng@2005-10-03.epub",
          "mediaType": "application/epub+zip"
        },
        {
          "rel": "toc",
          "title": "Table of Contents",
          "href": "https://api.laws.africa/v2/akn/za-wc033/act/by-law/2005/beaches/eng@2005-10-03/toc.json",
          "mediaType": "application/json"
        },
        {
          "rel": "media",
          "title": "Media",
          "href": "https://api.laws.africa/v2/akn/za-wc033/act/by-law/2005/beaches/eng@2005-10-03/media.json",
          "mediaType": "application/json"
        }
      ]
    }
  ]
}

```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

Supported content types: JSON, PDF, ePub, zip

{% api-method method="get" host="https://api.laws.africa" path="/v2/:frbr-uri.:format" %}
{% api-method-summary %}
A single expression
{% endapi-method-summary %}

{% api-method-description %}
This endpoint fetches details of a single work expression identified by a fully-formed FRBR URI.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="frbr-uri" type="string" required=true %}
Full FRBR URI for the work.
{% endapi-method-parameter %}

{% api-method-parameter name="format" type="string" required=false %}
Response format.
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}

{% api-method-headers %}
{% api-method-parameter name="Authentication" type="string" required=true %}
Authentication to identify you.
{% endapi-method-parameter %}
{% endapi-method-headers %}

{% api-method-query-parameters %}
{% api-method-parameter type="string" name="coverpage" %}
Should the response include a generated coverpage? Use 1 for true, anything else for false. Default: 1. HTML-only.
{% endapi-method-parameter %}

{% api-method-parameter name="media-url" type="string" required=false %}
The fully-qualified URL prefix to use when generating links to embedded media, such as images.
{% endapi-method-parameter %}

{% api-method-parameter name="resolver" type="string" required=false %}
The fully-qualified URL to use when resolving references to other Akoma Ntoso documents. Use `no` or `none` to disable. Defaults to using the Laws.Africa resolver. 
{% endapi-method-parameter %}

{% api-method-parameter name="standalone" type="number" required=false %}
If this is `1` , the response will be a complete HTML document, including CSS, that can stand on its own. Otherwise it will be an HTML fragment. Default: false. HTML-only.
{% endapi-method-parameter %}
{% endapi-method-query-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}
A single work expression in JSON format.
{% endapi-method-response-example-description %}

```javascript
{
  "url": "https://api.laws.africa/v2/akn/za-wc033/act/by-law/2005/beaches/eng@2005-10-03",
  "title": "By-Law relating to Beaches",
  "created_at": "2019-06-27T18:28:27.231180Z",
  "updated_at": "2019-06-27T18:28:27.481534Z",
  "country": "za",
  "locality": "wc033",
  "nature": "act",
  "subtype": "by-law",
  "year": "2005",
  "number": "beaches",
  "frbr_uri": "/akn/za-wc033/act/by-law/2005/beaches",
  "expression_frbr_uri": "/akn/za-wc033/act/by-law/2005/beaches/eng@2005-10-03",
  "publication_date": "2005-10-03",
  "publication_name": "Western Cape Provincial Gazette",
  "publication_number": "6303",
  "publication_document": {
    "url": "https://archive.opengazettes.org.za/archive/ZA-WC/2005/provincial-gazette-ZA-WC-no-6303-dated-2005-10-03.pdf",
    "filename": "za-wc033-act-by-law-2005-beaches-publication-document.pdf",
    "mime_type": "",
    "size": null
  },
  "commenced": false,
  "commencement_date": null,
  "commencing_work": null,
  "assent_date": null,
  "repeal": null,
  "parent_work": null,
  "expression_date": "2005-10-03",
  "language": "eng",
  "points_in_time": [
    {
      "date": "2005-10-03",
      "expressions": [
        {
          "title": "By-Law relating to Beaches",
          "language": "eng",
          "expression_date": "2005-10-03",
          "expression_frbr_uri": "/akn/za-wc033/act/by-law/2005/beaches/eng@2005-10-03",
          "url": "https://api.laws.africa/v2/akn/za-wc033/act/by-law/2005/beaches/eng@2005-10-03"
        },
        {
          "title": "Verordening insake Strande",
          "language": "afr",
          "expression_date": "2005-10-03",
          "expression_frbr_uri": "/akn/za-wc033/act/by-law/2005/beaches/afr@2005-10-03",
          "url": "https://api.laws.africa/v2/akn/za-wc033/act/by-law/2005/beaches/afr@2005-10-03"
        }
      ]
    }
  ],
  "amendments": [],
  "stub": false,
  "numbered_title": null,
  "taxonomies": null,
  "as_at_date": "2005-10-03",
  "links": [
    {
      "rel": "alternate",
      "title": "HTML",
      "href": "https://api.laws.africa/v2/akn/za-wc033/act/by-law/2005/beaches/eng@2005-10-03.html",
      "mediaType": "text/html"
    },
    {
      "rel": "alternate",
      "title": "Standalone HTML",
      "href": "https://api.laws.africa/v2/akn/za-wc033/act/by-law/2005/beaches/eng@2005-10-03.html?standalone=1",
      "mediaType": "text/html"
    },
    {
      "rel": "alternate",
      "title": "Akoma Ntoso",
      "href": "https://api.laws.africa/v2/akn/za-wc033/act/by-law/2005/beaches/eng@2005-10-03.xml",
      "mediaType": "application/xml"
    },
    {
      "rel": "alternate",
      "title": "PDF",
      "href": "https://api.laws.africa/v2/akn/za-wc033/act/by-law/2005/beaches/eng@2005-10-03.pdf",
      "mediaType": "application/pdf"
    },
    {
      "rel": "alternate",
      "title": "ePUB",
      "href": "https://api.laws.africa/v2/akn/za-wc033/act/by-law/2005/beaches/eng@2005-10-03.epub",
      "mediaType": "application/epub+zip"
    },
    {
      "rel": "toc",
      "title": "Table of Contents",
      "href": "https://api.laws.africa/v2/akn/za-wc033/act/by-law/2005/beaches/eng@2005-10-03/toc.json",
      "mediaType": "application/json"
    },
    {
      "rel": "media",
      "title": "Media",
      "href": "https://api.laws.africa/v2/akn/za-wc033/act/by-law/2005/beaches/eng@2005-10-03/media.json",
      "mediaType": "application/json"
    }
  ]
}

```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

Supported content types: JSON, HTML, PDF, ePub, zip

## Work expression attributes

The fields of the work and expression endpoints are described in the table below.

| Field | Description | Type |
| :--- | :--- | :--- |
| amendments | List of amendments that have been applied to create this expression of the work. | [See below](works-and-expressions.md#amendments) |
| assent\_date | Date when the work was assented to. | ISO8601 |
| content\_url | URL of the full content of the work. | URL |
| country | ISO 3166-1 alpha-2 country code that this work is applicable to. | String |
| created\_at | Timestamp of when the work was first created. | ISO8601 |
| draft | Is this a draft work or is it available in the public API? | Boolean |
| expression\_date | Date of this expression of the work. | ISO8601 |
| commencement\_date | Date on which this work commences. | ISO8601 |
| commencing\_work | Details of the work which commenced this work, if any. | Object |
| expression\_frbr\_uri | FRBR URI of this expression of this work. | String |
| frbr\_uri | FRBR URI for this work. | String |
| id | Unique ID of this work. | Integer |
| language | Three letter ISO-639-2 language code for this expression of the work. | String |
| links | A description of links to other formats of this expression that are available through the API. | Array |
| locality | The code of the locality within the country. | String |
| nature | The nature of this work, normally "act". | String |
| number | Number of this work with its year, or some other unique way of identifying it within the year. | String |
| parent\_work | The parent of this work. For subsidiary legislation, this is the principal legislation. | Object |
| points\_in\_time | Points in time that are available for this work. | [See below](works-and-expressions.md#points-in-time) |
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

The fields of the `amendments` property of the response are described below. These are the amendments that have been applied to produce this particular expression.

| Field | Description | Type |
| :--- | :--- | :--- |
| amending\_title | Title of the amending work | String |
| amending\_uri | Work FRBR URI of the amending work | String |
| date | Date on which the amendment takes place | ISO8601 |

#### Points in Time

The fields of the `points_in_time` property of the response are described below. These are all the available points in time available for this work.

| Field | Description | Type |
| :--- | :--- | :--- |
| date | Date of the point-in-time for which expressions are available | ISO8601 |
| expressions | A list of expressions for this work available at this point in time | List |
| url | The API URL to fetch information on the expression | URL |
| language | Three-letter language code of the language of the expression | String |
| expression\_frbr\_uri | Unique Expression FRBR URI for this expression | String |
| expression\_date | Date of this expression | ISO8601 |
| title | Title of the work, appropriate for the expression in the expression's language\) | String |

## Expressions at specific points in time

Works may be amended and change over time. You can fetch different amended versions of a work by specifying the language and date in the FRBR URI of the request.

The available points in time of a work are listed in the `points_in_time` field of the JSON description of the work. Each point in time includes a date and a list of expressions available at that date, one for each available language.

To fetch the very first expression of a work, use `frbr-uri/:language@`, for example: `/akn/za/act/1998/5/eng@`.

To fetch a specific point in time, use `frbr-uri/:language@:date`, for example: `/akn/za/act/1998/5/eng@2014-01-17`.

To fetch the most recent point in time at or before a specific date, use `frbr-uri/:language::date`, for example `/akn/za/act/1998/5/eng:2014-01-17`. 

{% hint style="info" %}
The `.format` part of the FRBR URI is placed after the `@YYYY-MM-DD` part.
{% endhint %}

{% hint style="info" %}
If you use `@` to specify a particular date and the API doesn't have a version at exactly that date, it will return a 404 response. If you need the expression of the work closest to a particular date, use `:` instead. 
{% endhint %}

### Date formats for specific points in time

| Date Format | Meaning | Example Expression FRBR URI |
| :--- | :--- | :--- |
| `@` | Very first expression of a work. | `/akn/za/act/1998/55/eng@` |
| `@YYYY-MM-DD` | Expression at the specific date. | `/akn/za/act/1998/55/eng@2014-01-17` |
| `:YYYY-MM-DD` | Most recent expression at or before a date. | `/akn/za/act/1998/55/eng:2015-01-01` |
| \(none\) | The most recent expression at or before today's date. Equivalent to using `:` with today's date. | `/akn/za/act/1998/55/eng` |

