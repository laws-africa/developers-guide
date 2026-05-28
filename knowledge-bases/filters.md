---
description: Filter Knowledge Base retrieve requests.
---

# Filters

Use filters to restrict which documents a Knowledge Base searches. Filters are most useful for legislation Knowledge
Bases, especially provincial and municipal collections.

## Recommended legislation filters

For most legislation queries, start with:

```json
{
  "filters": {
    "principal": true,
    "repealed": false
  }
}
```

These filters exclude amendment or commencement works and repealed legislation.

## Place filters

Use `frbr_place` or `frbr_place__in` when you know the country, province,
municipality or other locality you want to search.

```json
{
  "filters": {
    "frbr_place": "za-cpt"
  }
}
```

```json
{
  "filters": {
    "frbr_place__in": ["za-cpt", "za-jhb"]
  }
}
```

`frbr_place` is usually the safest place filter because it includes both the country and locality code.

## Other filters

The retrieve API also supports:

* `work_frbr_uri` and `work_frbr_uri__in`;
* `expression_frbr_uri` and `expression_frbr_uri__in`;
* `frbr_country`;
* `frbr_doctype` and `frbr_doctype__in`;
* `frbr_subtype` and `frbr_subtype__in`;
* `repealed`;
* `commenced`;
* `principal`.

Use exact filters only when your application already knows the relevant value. For exploratory search, start broad and
then narrow the query.

## Example

```json
{
  "text": "informal trading permit",
  "top_k": 5,
  "filters": {
    "principal": true,
    "repealed": false,
    "frbr_place": "za-cpt"
  }
}
```

See the [retrieve endpoint reference](reference/query-a-knowledge-base.md) for the full request schema.
