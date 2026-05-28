---
description: Work with legislation Knowledge Bases.
---

# Legislation Knowledge Bases

Legislation Knowledge Bases retrieve relevant portions of legislation, such as chapters, sections or schedules.

Use them when your product needs current legislative context for:

* AI grounding;
* legal search;
* legal agents;
* workflow triage;
* lightweight product integrations.

## What they return

Legislation results include:

* the matched legal text in `content.text`;
* work metadata such as `title`, `work_frbr_uri`, `frbr_place` and `expression_date`;
* portion metadata such as `portion_type`, `portion_id`, `portion_title` and `portion_public_url`;
* a public URL for the source work.

In some cases, a result may represent a page of a PDF when the legislation has not yet been converted into Akoma Ntoso
format.

## Recommended filters

For most legislation retrieval, use:

```json
{
  "principal": true,
  "repealed": false
}
```

Add a place filter when the user or workflow is about a specific jurisdiction:

```json
{
  "frbr_place": "za-cpt"
}
```

See [Filters](filters.md) for details.

## Versions

Knowledge Bases focus on the latest available legislation. Use the [Content API](../content-api/README.md) when your
product needs point-in-time versions or full local copies of legislation.
