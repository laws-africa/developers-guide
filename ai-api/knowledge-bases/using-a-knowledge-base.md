---
description: How to make use of Knowledge Base information
---

# Using a Knowledge Base

## Examples

Explore examples on how to use the Laws.Africa Knowledge Base API in our GitHub examples repo at [https://github.com/laws-africa/knowledge-base-examples](https://github.com/laws-africa/knowledge-base-examples)

## Using a legislation Knowledge Base

Often a user is not interested in the entirety of an Act, but only that portion which is applicable to their query. You can query a legislation Knowledge Base to find portions (chapters, sections) of legislation that match a query, and use those to help a user.

Let's assume we're building an LLM tool that looks for legislation to respond to a user's query, such as "How many dogs can I own in Cape Town"?

We're going to use this question directly to retrieve results from the Knowledge Base.

{% hint style="info" %}
&#x20;It's common to ask an LLM to generate an appropriate search based on a user's query, rather than use their query directly. This usually leads to better matches.
{% endhint %}

We're going to apply our query to the South African municipal by-laws Knowledge Base `legislation-za-municipal` to simplify our example.

We are going to include three filters to help ensure accurate results:

* `principal: true` so that we don't match on amendment or commencement works. Principal works are the main pieces of legislation.
* `repealed: false` so that we don't match repealed legislation which is no longer law.
* `frbr_place: "za-cpt"` so that we limit results to Cape Town. This is not strictly necessary if the query includes the words "Cape Town", but it can help.

See [#filtering-legislation-queries](using-a-knowledge-base.md#filtering-legislation-queries "mention") for more information on filters.

We're also going to include `top_k: 5` in my query to indicate I want the 5 best matches only.

```http
POST https://api.laws.africa/ai/v1/knowledge-bases/legislation-za-municipal/retrieve
Authorization: Token abc-123
Content-Type: application/json

{
  "text": "how many dogs can I own in Cape Town",
  "top_k": 5,
  "filters": {"principal": true, "repealed": false, "frbr_place": "za-cpt"}
}
```

The results look something like this (long lines and result list are truncated for readability):

```json
{
  "results": [
    {
      "content": {
        "text": "34. Offences and penalties (1) Any  person  who – (a) contravenes or fails to comply with any provision of this ..."
      },
      "metadata": {
        "work_frbr_uri": "/akn/za-cpt/act/by-law/2011/animal",
        "frbr_place": "za-cpt",
        "frbr_country": "za",
        "frbr_doctype": "act",
        "frbr_subtype": "by-law",
        "title": "Animal By-law, 2011",
        "repealed": false,
        "commenced": true,
        "principal": true,
        "blurb": null,
        "flynote": null,
        "expression_date": "2011-08-05",
        "expression_frbr_uri": "/akn/za-cpt/act/by-law/2011/animal/eng@2011-08-05",
        "public_url": "https://lawlibrary.org.za/akn/za-cpt/act/by-law/2011/animal/eng@2011-08-05",
        "portion_type": "provision",
        "portion_id": "chp_7",
        "portion_title": "Chapter 7 – Miscellaneous",
        "portion_parent_ids": null,
        "portion_parent_titles": null,
        "portion_public_url": "https://lawlibrary.org.za/akn/za-cpt/act/by-law/2011/animal/eng@2011-08-05#chp_7"
      },
      "score": 0.16151309999999997
    },
    { ... },
    { ... }
  ]
}
```

### Working with legislation results

The query returns 5 document portions that match my query, from two different by-laws. Each portion has metadata detailing the by-law it is from as well as information on the portion (type, title, id).

The score indicates the quality of the match (a lower score is better).

In the above example, the first match is from [Chapter 7](https://lawlibrary.org.za/akn/za-cpt/act/by-law/2011/animal/eng@2011-08-05#chp_7) of the Animal By-law, 2011 and the `content.text` field contains the full text of Chapter 7.

These results can be used in a [RAG system](https://en.wikipedia.org/wiki/Retrieval-augmented_generation) to provide context to an LLM to allow it to answer the user's query. Do this by building up a block of context to provide to the LLM:

1. group results by work (using `work_frbr_uri`)
2. add the work's title and public URL (so that the LLM can link the user to a page where they can read the by-law)
3. add the various portions returned for that work, including:
   1. the portion title (so the LLM can reference it)
   2. the portion public URL (so that the LLM can link the user to it)
   3. the text of the portion

You can then provide that content, and the user's question, to the LLM for it to provide an answer.

### Filtering legislation queries

See the **filters** parameter in [query-a-knowledge-base.md](query-a-knowledge-base.md "mention") for details of all filters that can be applied.

It is useful to apply filters to limit results for legislation queries. It is recommended you apply these filters for legislation:

* Principal works only (ie. exclude amending and commencement notices):  `principal: true`
* Exclude repealed works: `repealed: false`&#x20;

In JSON format, use:

```
"filters": {"principal": true, "repealed": false}
```

It can also be useful to use filters to limit results to a particular place, particularly when querying provincial or municipal Knowledge Bases. There are three filters that can be used, each with slightly different semantics. These filters are applied to the [work FRBR URI](../../get-started/works-and-expressions.md), so it is important to understand what that is.

<table><thead><tr><th width="184.32421875">Filter</th><th width="564.9140625">Meaning</th></tr></thead><tbody><tr><td><code>frbr_place</code><br><code>frbr_place__in</code><br><br></td><td>Limits the place (the combination of the country and locality codes). <strong>This is the recommended filtering method.</strong><br><br>Example:<br><br><code>{"frbr_place__in": ["za-cpt", "za-jhb"]</code><br><code>{"frbr_place": "za-cpt"}</code><br></td></tr><tr><td><code>frbr_country</code> </td><td>Limits the country using a <a href="https://en.wikipedia.org/wiki/ISO_3166-1_alpha-2">two-letter country code</a>. Has no impact on localities within the country (eg. provinces, municipalities).<br><br>Example:<br><br><code>{"frbr_country": "za"}</code></td></tr><tr><td><code>frbr_locality</code><br><code>frbr_locality__in</code></td><td>Limits the locality within the country, using country-specific codes. Locality codes between countries may not be unique.<br><br>Example:<br><br><code>{"frbr_locality": "cpt"}</code></td></tr></tbody></table>

## Using a judgment Knowledge Base

Let's assume we're building a tool to help a legal researcher find court judgments relevant to an issue they're dealing with. We can query the South African judgments knowledge base and use the summaries it returns to help the researcher discover relevant case law.

Let's say the user is looking for judgments related to delictual liability in a slip and trip case.

We're going to apply our query to the South African judgments Knowledge Base `judgments-za` . We're also going to include `top_k: 5` in my query to indicate I want the 5 best matches only.

We won't apply any filters.

```http
POST https://api.laws.africa/ai/v1/knowledge-bases/judgments-za/retrieve
Authorization: Token abc-123
Content-Type: application/json

{
  "text": "delict slip and trip",
  "top_k": 5
}
```

The results look something like this (long lines and result list are truncated for readability):

```json
{
  "results": [
    {
      "content": {
        "text": "The plaintiff sued for damages after a plank fell onto her foot while shopping in the defendant’s grocery store. ..."
      },
      "metadata": {
        "work_frbr_uri": "/akn/za-gp/judgment/zagpphc/2025/1051",
        "frbr_place": "za-gp",
        "frbr_country": "za",
        "frbr_doctype": "judgment",
        "frbr_subtype": "zagpphc",
        "title": "Matlou v Big Save (Pty) Ltd (22676/2019) [2025] ZAGPPHC 1051 (22 September 2025)",
        "repealed": null,
        "commenced": null,
        "principal": null,
        "blurb": "Occupier held 100% liable where unsecured plank fell on shopper; third‑party bump and disclaimers did not absolve defendant.",
        "flynote": "Delict — occupier’s duty to protect customers from foreseeable hazards; slip/fall — unsecured object falling when bumped by another user; intervening third party does not necessarily break causation; disclaimer notices and s 49 Consumer Protection Act — limitations of disclaimers in consumer premises; contributory negligence question and apportionment.",
        "expression_date": "2025-09-22",
        "expression_frbr_uri": "/akn/za-gp/judgment/zagpphc/2025/1051/eng@2025-09-22",
        "public_url": "https://lawlibrary.org.za/akn/za-gp/judgment/zagpphc/2025/1051/eng@2025-09-22",
        "portion_type": "summary",
        "portion_id": null,
        "portion_title": null,
        "portion_parent_ids": null,
        "portion_parent_titles": null,
        "portion_public_url": null
      },
      "score": 0.970713073
    },
    { ... },
    { ... }
  ]
}
```

### Working with judgment results

The query returns 5 judgments that match my query. Each judgment has judgment metadata and the portion type is set to `summary`. We know that the `context.text` field therefore contains a summary of the judgment.

The score indicates the quality of the match (a lower score is better).

In the above example, the first match is for the case [Matlou v Big Save (Pty) Ltd (22676/2019) \[2025\] ZAGPPHC 1051](https://lawlibrary.org.za/akn/za-gp/judgment/zagpphc/2025/1051/eng@2025-09-22).

The text is a summary of the case.

The `blurb` is a short, descriptive one-line summary of the judgment and `flynote` has key phrases for issues handled in the judgment.

These results can be used in a [RAG system](https://en.wikipedia.org/wiki/Retrieval-augmented_generation) to provide context to an LLM to allow it to answer the user's query. Do this by building up a block of context to provide to the LLM. For each judgment,

1. include the title and public URL (so that the LLM can link the user to a page where they can read the judgment)
2. include the blurb and flynote if necessary
3. add the full summary of the case from the `context.text` field

You can then provide that content, and the user's question, to the LLM for it to provide an answer.
