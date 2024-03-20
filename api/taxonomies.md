# Taxonomies

Taxonomies are used to categorise and group works. Taxonomies are made up of topics that form a tree structure. Each topic has a unique `slug`  which identifies the topic.

## List taxonomies

<mark style="color:green;">`GET`</mark> [`https://api.laws.africa/v3/taxonomy-topics`](https://api.laws.africa/v3/taxonomy-topics)

Lists taxonomy topics.

**Response body**

| Name       | Type   | Description               |
| ---------- | ------ | ------------------------- |
| `name`     | string | Name of the topic         |
| `slug`     | string | Unique code for the topic |
| `children` | list   | List of children (if any) |

**Response**

{% tabs %}
{% tab title="200" %}
```json
{
  "count": 1,
  "next": null,
  "previous": null,
  "results": [
    {
      "name": "Subject areas",
      "slug": "subject-areas",
      "id": 480,
      "children": [
        {
          "name": "Agriculture and Land",
          "slug": "subject-areas-agriculture-and-land",
          "id": 481,
          "children": []
        },
        {
          "name": "Arts and Culture",
          "slug": "subject-areas-arts-and-culture",
          "id": 482,
          "children": []
        }
      ]
    }
  ]
}
```
{% endtab %}
{% endtabs %}

## List work expressions in a taxonomy

<mark style="color:green;">`GET`</mark> [`https://api.laws.africa/v3/taxonomy-topic/<slug>/work-expressions`](https://api.laws.africa/v3/taxonomy-topics)

Lists the work expressions that are tagged with the topic identify by `<slug>`.

**Response body**

See [Works and expressions](works-and-expressions.md).
