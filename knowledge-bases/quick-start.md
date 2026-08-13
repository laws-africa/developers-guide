---
description: Make your first Laws.Africa Knowledge Base query.
---

# Quick start

This guide shows you how to query a Knowledge Base and use the result in an application.

You will:

1. create a platform account and API token;
2. list available Knowledge Bases;
3. query a Knowledge Base;
4. use the returned legal context in your application.

## Create an API token

1. Sign up at [https://platform.laws.africa/](https://platform.laws.africa/).
2. Get your API token from [https://platform.laws.africa/api-keys/](https://platform.laws.africa/api-keys/).

In the examples below, replace `<YOUR_AUTH_TOKEN>` with your token.

Knowledge Base retrieve calls count toward your account's Knowledge Base usage limits. You can monitor usage in the platform and read rate-limit guidance in [manage your plan and subscription](../get-started/manage-your-plan.md).

## List available Knowledge Bases

```bash
curl -H "Authorization: Bearer <YOUR_AUTH_TOKEN>" \
  https://api.laws.africa/ai/v1/knowledge-bases
```

The response includes Knowledge Base codes. Use a code in the retrieve endpoint.

## Query a legislation Knowledge Base

This example queries the South African municipal legislation Knowledge Base for Cape Town dog ownership rules.

```bash
curl -X POST \
  -H "Authorization: Bearer <YOUR_AUTH_TOKEN>" \
  -H "Content-Type: application/json" \
  -d '{
    "text": "dog ownership cape town",
    "top_k": 5,
    "filters": {
      "principal": true,
      "repealed": false,
      "frbr_place": "za-cpt"
    }
  }' \
  https://api.laws.africa/ai/v1/knowledge-bases/legislation-za-municipal/retrieve
```

The response contains matching legal portions:

```json
{
  "results": [
    {
      "content": {
        "text": "..."
      },
      "metadata": {
        "title": "Animal By-law, 2011",
        "work_frbr_uri": "/akn/za-cpt/act/by-law/2011/animal",
        "public_url": "https://lawlibrary.org.za/...",
        "portion_title": "Chapter 7 - Miscellaneous",
        "portion_public_url": "https://lawlibrary.org.za/...#chp_7"
      },
      "score": 0.16151309999999997
    }
  ]
}
```

{% hint style="info" %}
For legislation queries, start with `principal: true` and `repealed: false` so results prefer current principal legislation rather than amendment notices or repealed works.
{% endhint %}

## Use Result

The examples below make the same Knowledge Base request and format each result as source-linked context.

{% tabs %}
{% tab title="JavaScript" %}
```javascript
// Node.js 18+
const TOKEN = "<YOUR_AUTH_TOKEN>";
const KB_CODE = "legislation-za-municipal";

const response = await fetch(
  `https://api.laws.africa/ai/v1/knowledge-bases/${KB_CODE}/retrieve`,
  {
    method: "POST",
    headers: {
      Authorization: `Bearer ${TOKEN}`,
      "Content-Type": "application/json",
    },
    body: JSON.stringify({
      text: "dog ownership cape town",
      top_k: 5,
      filters: {
        principal: true,
        repealed: false,
        frbr_place: "za-cpt",
      },
    }),
    signal: AbortSignal.timeout(30_000),
  },
);

if (!response.ok) {
  throw new Error(`Request failed: ${response.status} ${response.statusText}`);
}

const { results } = await response.json();

const context = results.map((result) => {
  const metadata = result.metadata;
  const source = metadata.portion_public_url || metadata.public_url;

  return [
    `Title: ${metadata.title}`,
    `Source: ${source}`,
    `Text: ${result.content.text}`,
  ].join("\n");
});

console.log(context.join("\n\n"));
```
{% endtab %}

{% tab title="Python" %}
```python
import requests

TOKEN = "<YOUR_AUTH_TOKEN>"
KB_CODE = "legislation-za-municipal"

response = requests.post(
    f"https://api.laws.africa/ai/v1/knowledge-bases/{KB_CODE}/retrieve",
    headers={
        "Authorization": f"Bearer {TOKEN}",
        "Content-Type": "application/json",
    },
    json={
        "text": "dog ownership cape town",
        "top_k": 5,
        "filters": {
            "principal": True,
            "repealed": False,
            "frbr_place": "za-cpt",
        },
    },
    timeout=30,
)
response.raise_for_status()

results = response.json()["results"]

context = []
for result in results:
    metadata = result["metadata"]
    context.append(
        "\n".join(
            [
                f"Title: {metadata.get('title')}",
                f"Source: {metadata.get('portion_public_url') or metadata.get('public_url')}",
                f"Text: {result['content']['text']}",
            ]
        )
    )

print("\n\n".join(context))
```
{% endtab %}

{% tab title="Go" %}
```go
package main

import (
	"bytes"
	"encoding/json"
	"fmt"
	"io"
	"log"
	"net/http"
	"strings"
	"time"
)

const (
	token  = "<YOUR_AUTH_TOKEN>"
	kbCode = "legislation-za-municipal"
)

type retrieveResponse struct {
	Results []struct {
		Content struct {
			Text string `json:"text"`
		} `json:"content"`
		Metadata struct {
			Title            string `json:"title"`
			PortionPublicURL string `json:"portion_public_url"`
			PublicURL        string `json:"public_url"`
		} `json:"metadata"`
	} `json:"results"`
}

func main() {
	payload := map[string]any{
		"text":  "dog ownership cape town",
		"top_k": 5,
		"filters": map[string]any{
			"principal": true,
			"repealed":  false,
			"frbr_place": "za-cpt",
		},
	}

	body, err := json.Marshal(payload)
	if err != nil {
		log.Fatal(err)
	}

	url := fmt.Sprintf(
		"https://api.laws.africa/ai/v1/knowledge-bases/%s/retrieve",
		kbCode,
	)
	request, err := http.NewRequest(http.MethodPost, url, bytes.NewReader(body))
	if err != nil {
		log.Fatal(err)
	}
	request.Header.Set("Authorization", "Bearer "+token)
	request.Header.Set("Content-Type", "application/json")

	client := &http.Client{Timeout: 30 * time.Second}
	response, err := client.Do(request)
	if err != nil {
		log.Fatal(err)
	}
	defer response.Body.Close()

	if response.StatusCode < 200 || response.StatusCode >= 300 {
		message, _ := io.ReadAll(response.Body)
		log.Fatalf("request failed: %s: %s", response.Status, message)
	}

	var data retrieveResponse
	if err := json.NewDecoder(response.Body).Decode(&data); err != nil {
		log.Fatal(err)
	}

	context := make([]string, 0, len(data.Results))
	for _, result := range data.Results {
		source := result.Metadata.PortionPublicURL
		if source == "" {
			source = result.Metadata.PublicURL
		}

		context = append(context, fmt.Sprintf(
			"Title: %s\nSource: %s\nText: %s",
			result.Metadata.Title,
			source,
			result.Content.Text,
		))
	}

	fmt.Println(strings.Join(context, "\n\n"))
}
```
{% endtab %}

{% tab title="C#" %}
```csharp
using System.Net.Http.Headers;
using System.Net.Http.Json;
using System.Text.Json;

const string token = "<YOUR_AUTH_TOKEN>";
const string kbCode = "legislation-za-municipal";

using var client = new HttpClient
{
    Timeout = TimeSpan.FromSeconds(30),
};
client.DefaultRequestHeaders.Authorization =
    new AuthenticationHeaderValue("Bearer", token);

var payload = new
{
    text = "dog ownership cape town",
    top_k = 5,
    filters = new
    {
        principal = true,
        repealed = false,
        frbr_place = "za-cpt",
    },
};

using var response = await client.PostAsJsonAsync(
    $"https://api.laws.africa/ai/v1/knowledge-bases/{kbCode}/retrieve",
    payload
);
response.EnsureSuccessStatusCode();

using var document = JsonDocument.Parse(
    await response.Content.ReadAsStringAsync()
);

var context = new List<string>();
foreach (var result in document.RootElement.GetProperty("results").EnumerateArray())
{
    var metadata = result.GetProperty("metadata");

    string? source = null;
    if (
        metadata.TryGetProperty("portion_public_url", out var portionUrl)
        && portionUrl.ValueKind == JsonValueKind.String
    )
    {
        source = portionUrl.GetString();
    }

    if (string.IsNullOrEmpty(source))
    {
        source = metadata.GetProperty("public_url").GetString();
    }

    context.Add(
        $"Title: {metadata.GetProperty("title").GetString()}\n"
        + $"Source: {source}\n"
        + $"Text: {result.GetProperty("content").GetProperty("text").GetString()}"
    );
}

Console.WriteLine(string.Join("\n\n", context));
```
{% endtab %}
{% endtabs %}

Pass this context into your search interface, RAG prompt or agent response with the user's question. Always include source URLs so users can inspect the legal material.

## Next steps

* Learn the main [Knowledge Base concepts](concepts.md).
* Apply [filters](filters.md) for more precise legislation results.
* Use results in [RAG, search and agent workflows](use-in-apps.md).
