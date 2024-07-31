# Authentication

Calls to the API must be authenticated using your API token. To get a token,

1. Sign up for a free Laws.Africa account at [https://edit.laws.africa/accounts/login/](https://edit.laws.africa/accounts/login/).
2. Get your API token from [https://edit.laws.africa/accounts/profile/api/](https://edit.laws.africa/accounts/profile/api/).

Include your API token in your API calls by including an `Authorization` header:

```
Authorization: Token YOUR_AUTH_TOKEN
```

If you're logged into your Laws.Africa account, you can also browse the API using your web browser and visiting [https://api.laws.africa/ai/v1/knowledge-bases](https://api.laws.africa/v2/countries).
