# APIs overview

dbt platform | Starter, Enterprise, Enterprise+ⓘ

Accounts on the Starter, Enterprise, and Enterprise+ plans can query the dbt APIs.

dbt provides the following APIs:

* The [dbt Administrative API](https://docs.getdbt.com/docs/dbt-apis/admin-api.md) can be used to administrate a dbt account. It can be called manually or with [the dbt Terraform provider](https://registry.terraform.io/providers/dbt-labs/dbtcloud/latest).
* The [dbt Discovery API](https://docs.getdbt.com/docs/dbt-apis/discovery-api.md) can be used to fetch metadata related to the state and health of your dbt project.
* The [Semantic Layer APIs](https://docs.getdbt.com/docs/dbt-apis/sl-api-overview.md) provides multiple API options which allow you to query your metrics defined in the Semantic Layer.

If you want to learn more about webhooks, refer to [Webhooks for your jobs](https://docs.getdbt.com/docs/deploy/webhooks.md).

For request quotas and throttling behavior, refer to [API rate limits](https://docs.getdbt.com/docs/dbt-apis/rate-limits.md).

## How to Access the APIs[​](#how-to-access-the-apis "Direct link to How to Access the APIs")

dbt supports two types of API Tokens: [personal access tokens](https://docs.getdbt.com/docs/dbt-apis/user-tokens.md) and [service account tokens](https://docs.getdbt.com/docs/dbt-apis/service-tokens.md). Requests to the dbt APIs can be authorized using these tokens.
