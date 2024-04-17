---
description: Fetch a single work expression using an FRBR URI.
---

# Single work expression

## Fetch a single work expression

{% swagger src="https://api.laws.africa/v3/schema" path="/v3/{frbr_uri}" method="get" %}
[https://api.laws.africa/v3/schema](https://api.laws.africa/v3/schema)
{% endswagger %}

#### Query Parameters

| Name       | Type   | Description                                                                                                                                                                 |
| ---------- | ------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| coverpage  | string | Should the response include a generated coverpage? Use 1 for true, anything else for false. Default: 1. HTML-only.                                                          |
| media-url  | string | The fully-qualified URL prefix to use when generating links to embedded media, such as images.                                                                              |
| resolver   | string | The fully-qualified URL to use when resolving references to other Akoma Ntoso documents. Use `no` or `none` to disable. Defaults to using the Laws.Africa resolver.         |
| standalone | number | If this is `1` , the response will be a complete HTML document, including CSS, that can stand on its own. Otherwise it will be an HTML fragment. Default: false. HTML-only. |

{% tabs %}
{% tab title="200 A single work expression in JSON format." %}
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
{% endtab %}
{% endtabs %}

Supported content types: JSON, HTML, PDF, ePub, zip

## Work expression attributes

The fields of the work and expression endpoints are described in the table below.

| Field                 | Description                                                                                    | Type                                                 |
| --------------------- | ---------------------------------------------------------------------------------------------- | ---------------------------------------------------- |
| amendments            | List of amendments that have been applied to create this expression of the work.               | [See below](works-and-expressions.md#amendments)     |
| as\_at\_date          | Date up to which this work is known to be up-to-date.                                          | ISO8601                                              |
| assent\_date          | Date when the work was assented to.                                                            | ISO8601                                              |
| content\_url          | URL of the full content of the work.                                                           | URL                                                  |
| country               | ISO 3166-1 alpha-2 country code that this work is applicable to.                               | String                                               |
| created\_at           | Timestamp of when the work was first created.                                                  | ISO8601                                              |
| draft                 | Is this a draft work or is it available in the public API?                                     | Boolean                                              |
| expression\_date      | Date of this expression of the work.                                                           | ISO8601                                              |
| commencement\_date    | Date on which this work commences.                                                             | ISO8601                                              |
| commencing\_work      | Details of the work which commenced this work, if any.                                         | Object                                               |
| commencements         | Details of the commencements which apply to this work.                                         | [See below](works-and-expressions.md#commencements)  |
| expression\_frbr\_uri | FRBR URI of this expression of this work.                                                      | String                                               |
| frbr\_uri             | FRBR URI for this work.                                                                        | String                                               |
| id                    | Unique ID of this work.                                                                        | Integer                                              |
| language              | Three letter ISO-639-2 language code for this expression of the work.                          | String                                               |
| links                 | A description of links to other formats of this expression that are available through the API. | Array                                                |
| locality              | The code of the locality within the country.                                                   | String                                               |
| nature                | The nature of this work, normally "act".                                                       | String                                               |
| number                | Number of this work with its year, or some other unique way of identifying it within the year. | String                                               |
| parent\_work          | The parent of this work. For subsidiary legislation, this is the principal legislation.        | Object                                               |
| points\_in\_time      | Points in time that are available for this work.                                               | [See below](works-and-expressions.md#points-in-time) |
| publication\_date     | Date of original publication of the work.                                                      | ISO8601                                              |
| publication\_name     | Name of the publication in which the work was originally published.                            | String                                               |
| publication\_number   | Number of the publication in which the work was originally published.                          | String                                               |
| repeal                | Description of the repeal of this work, if it has been repealed.                               | See below                                            |
| subtype               | Subtype code of the work.                                                                      | String                                               |
| title                 | Short title of the work, in the appropriate language.                                          | String                                               |
| updated\_at           | Timestamp of when the work was last updated.                                                   | ISO8601                                              |
| url                   | URL for fetching details of this work.                                                         | URL                                                  |
| year                  | Year of the work.                                                                              | Stri                                                 |

### Amendments

The fields of the `amendments` property of the response are described below. These are the amendments that have been applied to produce this particular expression.

| Field           | Description                             | Type    |
| --------------- | --------------------------------------- | ------- |
| amending\_title | Title of the amending work              | String  |
| amending\_uri   | Work FRBR URI of the amending work      | String  |
| date            | Date on which the amendment takes place | ISO8601 |

### Commencements

The items in the `commencements` property describe the various commencements that apply to this work, and are the same for all expressions.

In most cases, either the work is not commenced or a single work commences all of the work's provisions. In some edge cases, different provisions commence at different times and these objects describe those events.

In some extreme cases, the work that performed the commencement, or the commencement date, might not be known.

| Field                 | Description                                                                                                               | Type    |
| --------------------- | ------------------------------------------------------------------------------------------------------------------------- | ------- |
| commencing\_title     | Title of the commencing work (optional)                                                                                   | String  |
| commencing\_frbr\_uri | FRBR URI of the commencing work (optional)                                                                                | String  |
| date                  | Date of the commencement event (optional)                                                                                 | ISO8601 |
| main                  | Is this considered the primary or main commencement event? This is usually the event that commences the bulk of the work. | Boolean |
| all\_provisions       | Does this event commence all provisions? If this is true, there will only be one of these commencement objects.           | Boolean |
| provisions            | A list of IDs of the provisions that are commenced by this event. This is always empty if `all_provisions` is true.       | List    |

### Points in Time

The fields of the `points_in_time` property of the response are described below. These are all the available points in time available for this work.

| Field                 | Description                                                                     | Type    |
| --------------------- | ------------------------------------------------------------------------------- | ------- |
| date                  | Date of the point-in-time for which expressions are available                   | ISO8601 |
| expressions           | A list of expressions for this work available at this point in time             | List    |
| url                   | The API URL to fetch information on the expression                              | URL     |
| language              | Three-letter language code of the language of the expression                    | String  |
| expression\_frbr\_uri | Unique Expression FRBR URI for this expression                                  | String  |
| expression\_date      | Date of this expression                                                         | ISO8601 |
| title                 | Title of the work, appropriate for the expression in the expression's language) | String  |

## Expressions at specific points in time

Works may be amended and change over time. You can fetch different amended versions of a work by specifying the language and date in the FRBR URI of the request.

The available points in time of a work are listed in the `points_in_time` field of the JSON description of the work. Each point in time includes a date and a list of expressions available at that date, one for each available language.

To fetch the very first expression of a work, use `frbr-uri/:language@`, for example: `/akn/za/act/1998/5/eng@`.

To fetch a specific point in time, use `frbr-uri/:language@:date`, for example: `/akn/za/act/1998/5/eng@2014-01-17`.

To fetch the most recent point in time at or before a specific date, use `frbr-uri/:language::date`, for example `/akn/za/act/1998/5/eng:2014-01-17`.&#x20;

{% hint style="info" %}
The `.format` part of the FRBR URI is placed after the `@YYYY-MM-DD` part.
{% endhint %}

{% hint style="info" %}
If you use `@` to specify a particular date and the API doesn't have a version at exactly that date, it will return a 404 response. If you need the expression of the work closest to a particular date, use `:` instead.&#x20;
{% endhint %}

### Date formats for specific points in time

| Date Format   | Meaning                                                                                          | Example Expression FRBR URI          |
| ------------- | ------------------------------------------------------------------------------------------------ | ------------------------------------ |
| `@`           | Very first expression of a work.                                                                 | `/akn/za/act/1998/55/eng@`           |
| `@YYYY-MM-DD` | Expression at the specific date.                                                                 | `/akn/za/act/1998/55/eng@2014-01-17` |
| `:YYYY-MM-DD` | Most recent expression at or before a date.                                                      | `/akn/za/act/1998/55/eng:2015-01-01` |
| (none)        | The most recent expression at or before today's date. Equivalent to using `:` with today's date. | `/akn/za/act/1998/55/eng`            |
