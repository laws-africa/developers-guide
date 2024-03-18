---
description: Getting the latest data from the Content API.
---

# Staying up to date

## In this section

* Getting recently updated content from the Content API
* Tracking deleted items

## Knowing when content has changed

Legislation changes over time. As new works and expressions are added to the Laws.Africa Content API, our app needs to be kept up to date.

When we run our `ingest_capetown_bylaws.py` command, it would be useful to ignore any content that hasn't changed, and only process content that has changed.

The Laws.Africa Content API includes an attribute `updated_at` which is a timestamp of the most recent change to that work and expression. We can use that to ignore content that hasn't changed.

Here are two ways of doing this:

1. When processing content from the Content API, compare the new `updated_at` with the `updated_at` of the work or expression already in your database. Only process the update if the new `updated_at` is more recent than your existing version.
2. Keep a timestamp in your database of the last time you processed content from the API. When processing new content, import anything with a `updated_at` field that is more recent than the timestamp from your database.

## Knowing when content has been removed

Sometimes content will be removed from the Content API, although this is rare. This usually happens when the FRBR URI for a document has to be changed.

There is no way to query the Content API for items that have been deleted. Instead, do the following:

1. Fetch a list of all expression FRBR URIs from the Content API.
2. Fetch a list of all expression FRBR URIs in your database.
3. Compare the lists, and delete anything that is in your database, but is no longer included in the Content API.
