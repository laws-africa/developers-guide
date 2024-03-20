---
description: Changes to the Laws.Africa API.
---

# Changelog

{% hint style="info" %}
Version V2 of the API is deprecated and will be disabled during 2024. Please migrate to V3 of the API.
{% endhint %}

## Differences between V2 and V3

There are only minor changes between V2 and V3 of the Content API.

* In V3, the `commencements` field for work expressions is removed and is available at its own URL `/v3/<frbr-uri>/commencements.json` . This was done for performance reasons because for some works `commencements` is very large, but the data is not always used.
* The taxonomy topics URL has moved from `/v2/taxonomy_topics` in V2 to `/v3/taxonomy-topics` in V3.

