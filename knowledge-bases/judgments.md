---
description: Work with judgment Knowledge Bases.
---

# Judgment Knowledge Bases

Judgment Knowledge Bases retrieve relevant case law context.

Use them when your product needs:

* case law discovery;
* legal research triage;
* judgment context for AI answers;
* semantic search over case law summaries.

## What they return

Judgment results include:

* a judgment summary in `content.text`;
* judgment metadata such as `title`, `work_frbr_uri`, `expression_date` and `public_url`;
* summary fields such as `blurb` and `flynote` when available;
* a match score.

{% hint style="warning" %}
Judgment Knowledge Bases return summaries and metadata. They do not return the full original judgment text through the
retrieve response.
{% endhint %}

## Example query

```json
{
  "text": "delict slip and trip",
  "top_k": 5
}
```

Use the returned `public_url` so users can inspect the source judgment.
