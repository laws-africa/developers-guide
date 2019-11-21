---
description: >-
  Listing the countries and localities (sub-country regions) that are available
  from the Content API.
---

# Countries and localities

All Laws.Africa content belongs to a country or a locality within a country. A locality is a jurisdiction within a country, such as a province or municipality. Together, they form a two-level hierarchy.

## Country and locality codes

Countries are identified using two-letter [ISO 3166-1 alpha-2](https://en.wikipedia.org/wiki/ISO_3166-1_alpha-2) country codes, such as `na` or `za`.

Localities are identified using a combination of the country code and a locality code specific to the country, such as `za-cpt`.

Locality codes are not well standardised and may vary between different countries. In South Africa, for example, municipalities are identified by the codes determined by the [South African Municipal Demarcation Board](http://www.demarcation.org.za/).

{% api-method method="get" host="https://api.laws.africa" path="/v2/countries.json" %}
{% api-method-summary %}
Get countries and localities
{% endapi-method-summary %}

{% api-method-description %}
This endpoint lists the countries and localities that Laws.Africa knows about. It includes links to the APIs for listing works for each country and locality.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-headers %}
{% api-method-parameter name="Authentication" type="string" required=true %}
Authentication token to identify you.
{% endapi-method-parameter %}
{% endapi-method-headers %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}
Paginated list list of countries, localities and API endpoints.
{% endapi-method-response-example-description %}

```javascript
{
  "count": 2,
  "next": null,
  "previous": null,
  "results": [
    {
      "code": "na",
      "name": "Namibia",
      "localities": [],
      "links": [
        {
          "rel": "works",
          "title": "Works",
          "href": "https://api.laws.africa/v2/akn/na/"
        },
      ]
    },
    {
      "code": "za",
      "name": "South Africa",
      "localities": [
        {
          "code": "wc033",
          "name": "Cape Agulhas",
          "frbr_uri_code": "za-wc033",
          "links": [
            {
              "rel": "works",
              "title": "Works",
              "href": "https://api.laws.africa/v2/akn/za-wc033/"
            }
          ]
        },
        {
          "code": "cpt",
          "name": "Cape Town",
          "frbr_uri_code": "za-cpt",
          "links": [
            {
              "rel": "works",
              "title": "Works",
              "href": "https://api.laws.africa/v2/akn/za-cpt/"
            }
          ]
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



