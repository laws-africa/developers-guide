---
description: >-
  Fetch metadata and files for images embedded in the content of a work
  expression.
---

# Embedded images

Some documents include images. The media API includes metadata for these images, and provides a mechanism to fetch the images from the API so that you can store them and serve them to your users when they view the content.

## List media files for a work expression

{% swagger src="../../.gitbook/assets/Laws.Africa Content API 2024-04-23.yaml" path="/v3/{frbr_uri}/media" method="get" %}
[Laws.Africa Content API 2024-04-23.yaml](<../../.gitbook/assets/Laws.Africa Content API 2024-04-23.yaml>)
{% endswagger %}

## Download a media file

{% swagger src="../../.gitbook/assets/Laws.Africa Content API 2024-04-23.yaml" path="/v3/{frbr_uri}/media/{filename}" method="get" %}
[Laws.Africa Content API 2024-04-23.yaml](<../../.gitbook/assets/Laws.Africa Content API 2024-04-23.yaml>)
{% endswagger %}
