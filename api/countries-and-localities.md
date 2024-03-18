---
description: >-
  Listing the countries and localities (sub-country regions) that are available
  from the Content API.
---

# Countries and localities

All Laws.Africa content belongs to a country or a locality within a country. A locality is a jurisdiction within a country, such as a province or municipality. Together, they form a two-level hierarchy.

## Country and locality codes

Countries are identified using two-letter [ISO 3166-1 alpha-2](https://en.wikipedia.org/wiki/ISO\_3166-1\_alpha-2) country codes, such as `na` or `za`.

Localities are identified using a combination of the country code and a locality code specific to the country, such as `za-cpt`.

Locality codes are not well standardised and may vary between different countries. In South Africa, for example, municipalities are identified by the codes determined by the [South African Municipal Demarcation Board](http://www.demarcation.org.za/).

{% swagger baseUrl="https://api.laws.africa" path="/v2/countries.json" method="get" summary="Get countries and localities" %}
{% swagger-description %}
This endpoint lists the countries and localities that Laws.Africa knows about. It includes links to the APIs for listing works for each country and locality.
{% endswagger-description %}

{% swagger-parameter in="header" name="Authentication" type="string" %}
Authentication token to identify you.
{% endswagger-parameter %}

{% swagger-response status="200" description="Paginated list list of countries, localities and API endpoints." %}
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
{% endswagger-response %}
{% endswagger %}

