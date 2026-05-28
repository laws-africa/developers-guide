---
description: Fetch full legislation content and metadata from Laws.Africa.
---

# Content API

The Content API is a read-only API for listing and fetching published versions of legislative works.

Use it when your product needs full legislation collections inside your own systems, including:

* Akoma Ntoso XML;
* HTML;
* PDF where available;
* table of contents JSON;
* publication, commencement, amendment and repeal metadata;
* point-in-time legislation versions;
* webhooks for content updates.

For AI grounding, search, RAG and legal agents, start with [Knowledge Bases](../knowledge-bases/README.md). Use the
Content API when retrieval is not enough and your product needs deeper control over storage,
processing or presentation.

## Endpoint

The latest version of the Content API is version 3:

```text
https://api.laws.africa/v3/
```

## What you can build

The Content API is best for:

* legal publishing systems;
* compliance infrastructure;
* internal legal databases;
* offline processing;
* analytics and classification pipelines;
* advanced legislation rendering.

## Content formats

Some API calls can return content in multiple formats. Specify the format by
placing `.format` at the end of the URL or by using the `Accept` header.

* `.json` or `Accept: application/json`: JSON
* `.xml` or `Accept: application/xml`: Akoma Ntoso XML
* `.html` or `Accept: text/html`: HTML
* `.epub` or `Accept: application/epub+zip`: ePUB
* `.pdf` or `Accept: application/pdf`: PDF
* `.zip` or `Accept: application/zip`: ZIP file with XML and media attachments

{% hint style="info" %}
Not all responses support all formats. Endpoint reference pages describe the formats available for each response.
{% endhint %}

## OpenAPI schema

* Download the OpenAPI schema from [https://api.laws.africa/v3/schema](https://api.laws.africa/v3/schema)
* Swagger UI: [https://api.laws.africa/v3/schema/swagger-ui](https://api.laws.africa/v3/schema/swagger-ui)
* Redoc: [https://api.laws.africa/v3/schema/redoc](https://api.laws.africa/v3/schema/redoc)
