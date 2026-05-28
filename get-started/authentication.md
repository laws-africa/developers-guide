---
description: Authenticate Laws.Africa API requests with an API token.
---

# Authentication and API keys

Calls to the Laws.Africa APIs must be authenticated with an API token.

1. Sign up for a Laws.Africa platform account at [https://platform.laws.africa/](https://platform.laws.africa/).
2. Create or copy your API token from [https://platform.laws.africa/api-keys/](https://platform.laws.africa/api-keys/).
3. Include the token in the `Authorization` header for API requests.

```http
Authorization: Bearer <YOUR_AUTH_TOKEN>
```

For example:

```bash
curl -H "Authorization: Bearer <YOUR_AUTH_TOKEN>" \
  https://api.laws.africa/ai/v1/knowledge-bases
```

{% hint style="info" %}
Keep your API token private. Do not commit it to source control or expose it in browser-side code.
{% endhint %}

If you are logged into your platform account, you can also browse some API endpoints directly in your web browser.
