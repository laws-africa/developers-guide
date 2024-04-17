---
description: >-
  List the places - countries and localities (sub-country regions) - that are
  available from the Content API.
---

# Places

All Laws.Africa content belongs to a country or a locality within a country. A locality is a jurisdiction within a country, such as a province or municipality. Together, they form a two-level hierarchy.

## Place codes

**Countries** are identified using two-letter [ISO 3166-1 alpha-2](https://en.wikipedia.org/wiki/ISO\_3166-1\_alpha-2) country codes, such as `na` or `za`.

**Localities** are identified using a combination of the country code and a locality code specific to the country, such as `za-cpt`.

Locality codes are not well standardised and may vary between different countries. In South Africa, for example, municipalities are identified by the codes determined by the [South African Municipal Demarcation Board](http://www.demarcation.org.za/).

## Get places

{% swagger src="https://api.laws.africa/v3/schema" path="/v3/places" method="get" %}
[https://api.laws.africa/v3/schema](https://api.laws.africa/v3/schema)
{% endswagger %}

## Get a place

{% swagger src="https://api.laws.africa/v3/schema" path="/v3/places/{frbr_uri_code}" method="get" %}
[https://api.laws.africa/v3/schema](https://api.laws.africa/v3/schema)
{% endswagger %}

## Get work expressions for a place

Use the `frbr_uri_code` for the place to fetch work expressions in that place.

{% swagger src="https://api.laws.africa/v3/schema" path="/v3/places/{frbr_uri_code}/work-expressions" method="get" %}
[https://api.laws.africa/v3/schema](https://api.laws.africa/v3/schema)
{% endswagger %}
