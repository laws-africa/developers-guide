# About the API

This describes the Laws.Africa Content API.

## Endpoint

The latest version of the API is version 3. It is available at the endpoint:

```javascript
https://api.laws.africa/v3/
```

## OpenAPI Schema

The API is described using the Open API format.

* Download the OpenAPI Schema from [https://api.laws.africa/v3/schema](https://api.laws.africa/v3/schema)
* API documentation in Swagger format: [https://api.laws.africa/v3/schema/swagger-ui](https://api.laws.africa/v3/schema/swagger-ui)
* API documentation in Redoc format: [https://api.laws.africa/v3/schema/redoc](https://api.laws.africa/v3/schema/redoc)

## Akoma Ntoso FRBR URIs

The API relies heavily on Akoma Ntoso FRBR URIs, which are described in the [Akoma Ntoso naming convention standard](http://docs.oasis-open.org/legaldocml/akn-nc/v1.0/akn-nc-v1.0.html).

When we use a URL such as `/v3/frbr-uri/` in this guide, the `frbr-uri` part is a full FRBR URI, such as `/akn/za/act/1998/84/eng`.

## Content types

Some API calls can return content in multiple formats. You can specify the required content of your request by placing `.format` at the end of the URL. In most cases the default response type is JSON.

* `.json` or `Accept: application/json`: return JSON
* `.xml` or `Accept: application/xml`: return Akoma Ntoso XML
* `.html` or `Accept: text/html`: return human friendly HTML
* `.epub` or `Accept: application/epub+zip`: return an ePUB (ebook) document
* `.pdf` or `Accept: application/pdf`: return a PDF document
* `.zip` or `Accept: application/zip`: return a ZIP file with the document XML and media attachments

{% hint style="info" %}
Not all responses support all formats, the documentation will be explicit about what is supported.
{% endhint %}
