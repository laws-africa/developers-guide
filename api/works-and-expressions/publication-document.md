---
description: Fetch the details of the original publication document for a work.
---

# Publication document

The original publication document is available for most works. This is the primary source document for the work, such as the official government gazette. It is always a PDF.

{% hint style="info" %}
The original publication document is different to the PDF version of a work expression.

* The **PDF version** is a PDF version of the XML content of the document that is generated automatically.
* The **publication document PDF** is a copy of the original publication document, which may be a scanned PDF. It is usually only the original version of the document and does not contain amendment information.
{% endhint %}

## Download the publication document for a work

The full URL to download the publication document is part of the `publication_document` field in the details of the work.

{% swagger src="../../.gitbook/assets/Laws.Africa Content API 2024-04-23.yaml" path="/v3/{frbr_uri}/media/publication/{filename}" method="get" %}
[Laws.Africa Content API 2024-04-23.yaml](<../../.gitbook/assets/Laws.Africa Content API 2024-04-23.yaml>)
{% endswagger %}
