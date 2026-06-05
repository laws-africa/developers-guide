---
description: Manage your Laws.Africa platform plan, subscription, usage and service access.
---

# Manage your plan and subscription

Use [platform.laws.africa](https://platform.laws.africa/) to manage the account
settings that control what your API token can access.

The developer guide explains how to integrate with the APIs. The platform is
where you manage your API token, active services, selected country, usage, plan
and subscription requests.

## Check your current plan

Open [Plan & billing](https://platform.laws.africa/plan-billing/) to see:

* your current plan;
* your primary API country;
* your active services;
* your API rate limits;
* pending upgrade, downgrade or cancellation requests.

Use [Usage](https://platform.laws.africa/usage/) to monitor how many calls your
account has made against your current limits.

## Primary API country

Your primary API country controls the default country-scoped services on
self-service plans, including country-specific Knowledge Bases.

If your plan allows country changes, you can change your primary API country
from [Plan & billing](https://platform.laws.africa/plan-billing/). Changing the
country can change which country-specific services are active for your account.

## Change your plan

Plan changes are managed from
[Plan & billing](https://platform.laws.africa/plan-billing/).

Depending on your current plan, you may be able to:

* request an upgrade to Build;
* contact Laws.Africa about Scale;
* request a downgrade;
* request cancellation.

Some changes are handled as requests rather than instant changes, especially
where they affect countries, products, billing or commercial terms. Pending
requests are shown on the Plan & billing page.

For current prices and commercial plan details, use
[laws.africa/platform](https://laws.africa/platform/).

## Monitor usage and handle rate limits

API calls are counted against your account's active plan limits. Limits may
apply per minute and per day, depending on the product family and your plan.

Open [Usage](https://platform.laws.africa/usage/) to check current usage.

Your application should handle `429 Too Many Requests` responses gracefully. For
production integrations:

* log API response status codes;
* use retry and backoff for rate-limit responses;
* avoid tight retry loops;
* cache stable Content API responses where appropriate;
* monitor daily usage trends before launch;
* request an upgrade before sustained usage reaches your plan limits.

If usage looks unexpectedly high, check for repeated polling, unbounded loops,
duplicate background jobs or repeated uncached requests.

## Related account tasks

* Manage API keys at
  [https://platform.laws.africa/api-keys/](https://platform.laws.africa/api-keys/).
* Review [developer entry points](https://platform.laws.africa/developer-resources/).
* When logged in, manage Content API webhooks at
  [https://platform.laws.africa/webhooks/](https://platform.laws.africa/webhooks/).
