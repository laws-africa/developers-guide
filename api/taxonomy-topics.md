---
description: Taxonomies are used to classify works.
---

# Taxonomy topics

Taxonomies are used to categorise and group works. Taxonomies are made up of topics that form a tree structure. Each topic has a unique `slug`  which identifies the topic.

A work may be associated with zero, one or many taxonomy topics.

## List taxonomy topics

{% swagger src="../.gitbook/assets/Laws.Africa Content API 2024-04-23.yaml" path="/v3/taxonomy-topics" method="get" %}
[Laws.Africa Content API 2024-04-23.yaml](<../.gitbook/assets/Laws.Africa Content API 2024-04-23.yaml>)
{% endswagger %}

## Get a taxonomy topic

{% swagger src="../.gitbook/assets/Laws.Africa Content API 2024-04-23.yaml" path="/v3/taxonomy-topics/{slug}" method="get" %}
[Laws.Africa Content API 2024-04-23.yaml](<../.gitbook/assets/Laws.Africa Content API 2024-04-23.yaml>)
{% endswagger %}

## List work expressions tagged with a taxonomy topic

{% swagger src="../.gitbook/assets/Laws.Africa Content API 2024-04-23.yaml" path="/v3/taxonomy-topics/{slug}/work-expressions" method="get" %}
[Laws.Africa Content API 2024-04-23.yaml](<../.gitbook/assets/Laws.Africa Content API 2024-04-23.yaml>)
{% endswagger %}
