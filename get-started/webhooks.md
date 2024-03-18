---
description: Webhooks are push notifications when a work is created, updated or deleted.
---

# Webhooks

You can add a webhook URL in your accounts page on Laws.Africa at [https://edit.laws.africa/accounts/profile/webhooks](https://edit.laws.africa/accounts/profile/webhooks).

When a Work Expression is created, updated or deleted, [Laws.Africa](http://laws.africa) will send a POST request to the webhook URL with details of the action in JSON in the body of the request.

## Add a webhook

1. Visit [https://edit.laws.africa/accounts/profile/webhooks](https://edit.laws.africa/accounts/profile/webhooks)
2. Under **Add a new webhook** fill in the URL field with your webhook URL.
3. Click **Add webhook**

<figure><img src="../.gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>

## Delete a webhook

1. Visit [https://edit.laws.africa/accounts/profile/webhooks](https://edit.laws.africa/accounts/profile/webhooks)
2. Click the **Delete** button alongside the webhook you wish to delete.

## Webhook invocation details

The body of a webhook invocation request contains the following information:

```jsx
{
  "invocation_id": "abc-123", // unique ID for this invocation; re-used if the invocation fails and is retried
  "request_id": "def-456",    // unique request ID for this HTTP request, never re-used
  "action": "updated",        // or "created" or "deleted"
  "object": "work",
  "work": {
    "frbr_uri": "/akn/za/act/2009/1",
    "url": "https://api.laws.africa/v3/akn/za/act/2009/1"
  }
}
```

## Handling a webhook invocation

A webhook invocation should be treated as an indication that the work has been changed or deleted. It should do minimal work so that it can respond within the 10 second timeout. A common approach is to trigger an asynchronous (background) task to fetch/refresh the data for all expressions of the work, and return a 200-success response.

### Retries

[Laws.Africa](http://laws.africa) expects the webhook endpoint to return a 200-level response code within 10 seconds. A response that takes longer than that is considered a failure.

Laws.Africa will retry a failed (non-200 response) webhook invocation up to 10 times. Laws.Africa will initially retry after just a few seconds. If more attempts fail, it will use exponential backoff to delay retrying longer and longer (up to a few hours between attempts).

### Duplicate invocations

It is possible for a work to be updated rapidly in quick succession. [Laws.Africa](http://laws.africa) will group multiple updates to the same work within a short period (30 seconds) into a single invocation. This avoids overloading the webhook target with repeated invocations in quick succession.

Because a webhook invocation can be retried, it is important that your webhook endpoint can safely process a duplicate invocation. [Laws.Africa](http://laws.africa) cannot guarantee exactly-once delivery of a webhook invocation.

### Handling deletions

In rare cases a work may be deleted and then re-created a few minutes later. The order of the webhook invocations is not guaranteed. This means that you may receive the “deleted” webhook invocation after the “created” invocation (this is the joy of distributed systems across the Internet!).

Therefore, rather than immediately deleting a work when getting a “deleted” webhook, first call the [Laws.Africa](http://laws.africa) API to see if that work still exists. If the API returns a 404, then it is safe to delete that work. If the work does exist, then consider your copy out of date and treat the invocation as an “updated” invocation.
