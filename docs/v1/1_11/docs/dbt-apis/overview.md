# APIs overview

dbt platform | Starter, Enterprise, Enterprise+ⓘ

Accounts on the Starter, Enterprise, and Enterprise+ plans can query the dbt APIs.

dbt provides the following APIs:

* The [dbt Administrative API](./admin-api.md) can be used to administrate a dbt account. It can be called manually or with [the dbt Terraform provider](https://registry.terraform.io/providers/dbt-labs/dbtcloud/latest).
* The [dbt Discovery API](./discovery-api.md) can be used to fetch metadata related to the state and health of your dbt project.
* The [Semantic Layer APIs](./sl-api-overview.md) provides multiple API options which allow you to query your metrics defined in the Semantic Layer.

If you want to learn more about webhooks, refer to [Webhooks for your jobs](../deploy/webhooks.md).

For request quotas and throttling behavior, refer to [API rate limits](./rate-limits.md).

## How to Access the APIs[​](#how-to-access-the-apis "Direct link to How to Access the APIs")

dbt supports two types of API Tokens: [personal access tokens](./user-tokens.md) and [service account tokens](./service-tokens.md). Requests to the dbt APIs can be authorized using these tokens.
