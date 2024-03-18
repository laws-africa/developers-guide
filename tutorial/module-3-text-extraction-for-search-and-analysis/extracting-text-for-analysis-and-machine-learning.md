---
description: >-
  How to extract text from all portions of a document, for use in analysis or
  machine learning.
---

# Extracting text for analysis and machine learning

## In this section

* Using the Table of Contents to extract text recursively from an entire document
* Extracting text only from the lowest hierarchical element

## Extracting text from different portions of a document

Sometimes it's useful to break a piece of legislation down into its constituent portions (sections, paragraphs, sub-sections, etc.) and extract the text from each of those parts and work with them individually, rather than the entire document as a whole.

Example use cases include:

* Semantic search or document similarity: using language embedding models to calculate embeddings for different parts of the document.
* Keyword and taxonomy tagging: using machine learning models to automatically apply taxonomy tags to different parts of a document.

## Recursive text extract using the Table of Contents

In this section, we'll recursively extract text from the different portions of the document, following the hierarchy defined by the Table of Contents.

### Fetch the Table of Contents (TOC)

The Laws.Africa Content API can provide the Table of Contents (TOC) hierarchy of a document in JSON format. This is often easier than trying to build it yourself.

Let's fetch it from the API for our example Cape Town Liquor By-law, using the same API token we used in [basics-of-text-extraction.md](basics-of-text-extraction.md "mention").

```python
import json

url = "https://api.laws.africa/v3/akn/za-cpt/act/by-law/2014/control-undertakings-liquor/eng/toc.json"
request = Request(url, headers={"Authorization": f"Token {TOKEN}"})
toc = json.loads(urlopen(request).read())['toc']
toc
# [{'type': 'preface',
# 'component': 'main',
# 'title': 'Preface',
# 'children': [],
# 'basic_unit': False,
# 'num': None,
# ....
```

The `toc` variable now contains an array of TOC entries. Each entry has key details such as a type, a title, and an `id`. The `id` matches the **eId** of the corresponding XML element.

A TOC item can also have nested items in its `children` attribute.

### Extract text from TOC entries

Let's create a function that recursively extracts the text from each entry in the TOC, as follows:

1. iterate over each TOC entry, and its children
2. for each entry, get the corresponding XML element from the XML document tree
3. extract the text from the element, and store it on a new `text` attribute on the TOC entry

```python
# setup a namespace, as before
# we assume that we have already parsed the document xml into the variable "root"
ns = root.nsmap[None]
nsmap = {"a": ns}

def extract_item_text(item):
  if item['id']:
    # get the XML element with this id
    elements = root.xpath(f'//a:*[@eId="{item["id"]}"]', namespaces=nsmap)
    if elements:
      el = elements[0]
      # get text nodes from this element, ignoring editorial remarks
      item['text'] = ' '.join(el.xpath('.//text()[not(ancestor::a:remark)]', namespaces=nsmap))
  
  # recursively extract text from the children
  for child in item['children']:
    extract_item_text(child)
    
# for each item in the toc, add a "text" attribute with the text of that item
for item in toc:
  extract_item_text(item)

toc[3]
# {'type': 'section',
# 'component': 'main',
# 'title': '2. Application',
# 'children': [],
# 'basic_unit': True,
# 'num': '2.',
# 'id': 'sec_2',
# 'heading': 'Application',
# 'url': 'https://api.laws.africa/v3/akn/za-cpt/act/by-law/2014/control-undertakings-liquor/eng/!main~sec_2',
# 'text': '2. Application This By-law is applicable to licensees that  sell   liquor  to the public within the jurisdiction of the  City .'
# }
```

We now have the text of the entire document broken down into individual portions.

With this information we can:

* Calculate a text embedding for the **text** of each portion of the document, and index it into a vector database along with the **heading** and **id** of the portion. We can then run a semantic search query, and return the text, heading and id of matching portions.
* Apply taxonomy tags to the text of each portion of the document, using a tool like Pool Party. We can then store the resulting **tags** and the **id** of the portion, and use it to enrich the document as shown in [advanced-enrichments.md](../module-2-enrichments-and-interactivity/advanced-enrichments.md "mention").
