---
description: Fetch a single work expression using an FRBR URI.
---

# Single work expression

## Fetch a single work expression

{% swagger src="../.gitbook/assets/Laws.Africa Content API 2024-04-23.yaml" path="/v3/{frbr_uri}" method="get" %}
[Laws.Africa Content API 2024-04-23.yaml](<../.gitbook/assets/Laws.Africa Content API 2024-04-23.yaml>)
{% endswagger %}

#### Query Parameters

| Name       | Type   | Description                                                                                                                                                                 |
| ---------- | ------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| coverpage  | string | Should the response include a generated coverpage? Use 1 for true, anything else for false. Default: 1. HTML-only.                                                          |
| media-url  | string | The fully-qualified URL prefix to use when generating links to embedded media, such as images.                                                                              |
| resolver   | string | The fully-qualified URL to use when resolving references to other Akoma Ntoso documents. Use `no` or `none` to disable. Defaults to using the Laws.Africa resolver.         |
| standalone | number | If this is `1` , the response will be a complete HTML document, including CSS, that can stand on its own. Otherwise it will be an HTML fragment. Default: false. HTML-only. |

Supported content types: JSON, HTML, PDF, ePub, zip

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
