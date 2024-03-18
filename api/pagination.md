# Pagination

API calls that return lists will be paginated and return a limited number of items per page. The response includes information on the total number of items and the URLs to use to fetch the next and previous pages of items:

<table><thead><tr><th width="141">Field</th><th width="487">Meaning</th><th>Type</th></tr></thead><tbody><tr><td><code>count</code></td><td>Total number of items in the entire response, across all pages.</td><td>number</td></tr><tr><td><code>next</code></td><td>URL for the next page of results, if any.</td><td>string or null</td></tr><tr><td><code>previous</code></td><td>URL for the previous page of results, if any.</td><td>string or null</td></tr></tbody></table>

Here's an example of the first page of a paginated response with 250 total items and two pages:

```javascript
{
  "count": 250,
  "next": "https://api.laws.africa/v3/akn/za.json?page=2",
  "previous": null,
  "results": [ "..." ]
}
```

In this case, fetching the `next` URL will return the second (and final) page.

Our recommended way of walking through all paginated results is using a `while` loop like the following in Python:

```python
def fetch(url):
  while url:
    response = get(url)
    results = response["results"]
    # TODO: do something with the results 
    url = response["next"]  
```
