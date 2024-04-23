---
description: Enrichment datasets add additional detail to provisions of a work.
---

# Enrichment datasets

The provisions (chapters, sections, paragraphs, etc.) of a work can be enriched with additional information. This information is stored separately to the content of the provision.

{% hint style="info" %}
Explore the tutorial for working with enrichment datasets.

[module-2-enrichments-and-interactivity](../tutorial/module-2-enrichments-and-interactivity/ "mention")
{% endhint %}

These enrichments are grouped into **enrichment datasets**. An enrichment dataset contains multiple enrichments for multiple works.

An enrichment dataset has a **root taxonomy topic**. This is the root of the taxonomy tree that the enrichment dataset uses. Provisions enriched by the dataset can be tagged with a taxonomy topic that is part of the enrichment dataset's taxonomy tree.

An enrichment is made up of:

* the work being enriched
* the eId of the provision being enriched
* the enrichment data:
  * a topic in the taxonomy topic tree associated with the enrichment dataset

{% hint style="info" %}
An enrichment applies to a provision across all expressions of a work.
{% endhint %}

## List enrichment datasets

{% swagger src="../.gitbook/assets/Laws.Africa Content API 2024-04-23.yaml" path="/v3/enrichment-datasets" method="get" %}
[Laws.Africa Content API 2024-04-23.yaml](<../.gitbook/assets/Laws.Africa Content API 2024-04-23.yaml>)
{% endswagger %}

## Get details of an enrichment dataset

{% swagger src="../.gitbook/assets/Laws.Africa Content API 2024-04-23.yaml" path="/v3/enrichment-datasets/{id}" method="get" %}
[Laws.Africa Content API 2024-04-23.yaml](<../.gitbook/assets/Laws.Africa Content API 2024-04-23.yaml>)
{% endswagger %}
