# API rate limits

dbt platform | Starter, Enterprise, Enterprise+

dbt applies request limits across the dbt platform APIs so integrations stay reliable and predictable.

The [Administrative API](./admin-api.md) and the [Discovery API](./discovery-api.md) do not share the same limit. You can send more requests per minute to admin endpoints under `/api/` and fewer to Discovery's GraphQL endpoints under `/graphql/`, because Discovery queries often return more data per call.

You use the same credentials for both APIs: [personal access tokens](./user-tokens.md), [service account tokens](./service-tokens.md), and OAuth when your organization supports it. Refer to [Authentication tokens](./authentication.md) to create and use those credentials, and the [APIs overview](./overview.md) to learn what each API does.

This page summarizes the default rate limits across the main API and integration surfaces:

* [Administrative API](./admin-api.md): 5,000 requests per minute per account (`/api/`).
* [Discovery API](./discovery-api.md) (GraphQL): 500 requests per minute (`/graphql/` on the `metadata` host).
* [Semantic Layer APIs](#semantic-layer-apis): Limited separately and not governed by the Discovery API limit.
* [SCIM and IdP provisioning](#scim-and-idp-provisioning): 20 requests every 5 seconds per account.
* [Remote MCP](#remote-mcp): 5,000 requests per minute per IP (global API rate limit).
* [Self-hosted MCP](#local-mcp): Uses the Administrative and Discovery API limits above.

For SCIM scope, throttling responses, and identity provider behavior, see [SCIM and IdP provisioning](#scim-and-idp-provisioning) and [Set up SCIM](../platform/manage-access/scim.md). For MCP integration and usage patterns, see [Remote MCP](#remote-mcp) and [Self-hosted MCP](#local-mcp).

Discovery API and Semantic Layer GraphQL endpoints

The Discovery API limit applies only to the metadata endpoint. The [Semantic Layer GraphQL API](./sl-graphql.md) uses a different host and has its own rate limiting.

## Pagination and the Discovery API

The most common way teams hit the Discovery API limit is to request very small pages in GraphQL (for example, `first: 1`) repeatedly. Each page is another request, so small pages require far more calls than fetching the same data in larger chunks. Use the largest page size that still works for you, up to what that query allows (often up to `500`). Full examples, including how to page with `after`, are in [Query the Discovery API](./discovery-querying.md).

## SCIM and IdP provisioning [Enterprise](https://www.getdbt.com/pricing "Go to https://www.getdbt.com/pricing")[Enterprise +](https://www.getdbt.com/pricing "Go to https://www.getdbt.com/pricing")

For SCIM, the application rate limit of 20 requests every 5 seconds per account (Enterprise and Enterprise+) applies only to creating a user (`POST /api/v3/accounts/{account_id}/scim/v2/Users`) and replacing a user (`PUT /api/v3/accounts/{account_id}/scim/v2/Users/{user_id}`). dbt counts requests in a fixed 5-second window.

It does not apply to `GET`, `PATCH`, or `DELETE` requests for users; to any Group operations (create, list, get, replace, patch, or delete); or to other SCIM endpoints such as Service Provider Config, schemas, resource types, or config token routes. Those operations are outside this limit but may still be subject to other platform rate limits.

When you exceed that limit, dbt returns `429 Too Many Requests` with both `Retry-After` and `x-rate-limit-retry-after-seconds` (for example, Okta integrations often honor `Retry-After`, while SailPoint often honors `x-rate-limit-retry-after-seconds`). These headers indicate how long to wait before you send the next request. Configure your identity provider to read the header your provisioning stack supports, and to retry with exponential backoff when throttled.

For configuration steps, use [Set up SCIM](../platform/manage-access/scim.md). For information on licenses, permissions, [SSO](../platform/manage-access/sso-overview.md), and how SCIM fits into account access, refer to [About user access in dbt](../platform/manage-access/about-user-access.md).

## Remote MCP

[Remote MCP](../dbt-ai/about-mcp.md) uses the same default global API rate limit as other dbt APIs: 5,000 requests per minute per IP. It uses the same authentication as other integrations (for example, personal access tokens, service account tokens, or OAuth where supported).

Treat remote MCP automation like any other API client: avoid retrying without pausing between attempts. When you receive a `429` response, wait before trying again, and wait longer between retries if you continue to receive `429` responses.

## Semantic Layer APIs

dbt rate limits the [Semantic Layer APIs](./sl-api-overview.md) (GraphQL, JDBC, and Python SDK) independently of the Administrative API and Discovery API. The Semantic Layer limit is shared across all clients, users, and accounts in the same instance (for example, every account in US1 shares a single limit), so your own request volume isn't the only factor that determines whether you hit it.

## Self-hosted MCP

[Self-hosted MCP](../dbt-ai/about-mcp.md) calls the public [Administrative API](./admin-api.md) and [Discovery API](./discovery-api.md) directly, so those limits apply: 5,000 requests per minute per account for `/api/` and 500 requests per minute for `/graphql/`, as summarized above.

## Exceeding the rate limit

For the [Administrative API](./admin-api.md) and [Discovery API](./discovery-api.md), if you exceed the limit, dbt returns `429 Too Many Requests` and enforces a five-minute cooldown. After five minutes, you can send requests again as usual.

This cooldown doesn't apply to the Semantic Layer API.

### Exceeding the rate limit for the Semantic Layer API

If you receive a `429 Too Many Requests` error, you exceeded the Semantic Layer limit. There's no cooldown period, so requests succeed again as soon as shared usage drops back under the limit. To reduce your own request footprint:

* Batch multiple metrics into a single query.
* Retry with exponential backoff and jitter instead of polling at a fixed interval.
* Use preset date ranges instead of custom ranges so dbt can reuse cached results.
* Pre-materialize metrics that can be pre-aggregated by using [exports](../use-dbt-semantic-layer/exports.md) or [saved queries](../build/saved-queries.md). Metrics that can't be pre-aggregated, such as distinct counts, still run live.

These steps reduce your own usage but might not prevent every `429` error if other clients share the same limit. For dedicated options, contact your account team.
