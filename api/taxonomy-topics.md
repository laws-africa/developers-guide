---
description: Taxonomies are used to classify works.
---

# Taxonomy topics

Taxonomies are used to categorise and group works. Taxonomies are made up of topics that form a tree structure. Each topic has a unique `slug`  which identifies the topic.

A work may be associated with zero, one or many taxonomy topics.

## List taxonomy topics

{% swagger src="https://api.laws.africa/v3/schema" path="/v3/taxonomy-topics" method="get" %}
[https://api.laws.africa/v3/schema](https://api.laws.africa/v3/schema)
{% endswagger %}

## Get a taxonomy topic

{% swagger src="https://api.laws.africa/v3/schema" path="/v3/taxonomy-topics/{slug}" method="get" %}
[https://api.laws.africa/v3/schema](https://api.laws.africa/v3/schema)
{% endswagger %}

## List work expressions tagged with a taxonomy topic

{% swagger src="https://api.laws.africa/v3/schema" path="/v3/taxonomy-topics/{slug}/work-expressions" method="get" %}
[https://api.laws.africa/v3/schema](https://api.laws.africa/v3/schema)
{% endswagger %}
