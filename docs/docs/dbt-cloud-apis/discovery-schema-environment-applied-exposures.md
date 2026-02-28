# Exposures object schema

[Exposures](https://docs.getdbt.com/docs/build/exposures.md) are dbt resources that represent downstream uses of your project, such as dashboards, applications, or data science pipelines. You can query exposures through the Discovery API to understand which assets depend on your models.

The [Example query](#example-query) illustrates a few fields you can query with the `exposures` object. Refer to [Fields](#fields) to view the entire schema, which provides all possible fields you can query.

### Arguments[​](#arguments "Direct link to Arguments")

When querying for `exposures`, you can use the following arguments:

# Fetching data...

### Example query[​](#example-query "Direct link to Example query")

You can specify the `environmentId`, `first: 100`, and filter by model `uniqueIds` to return all the downstream exposures (dashboards, applications, etc.) that depend on the `customers` model in the `marketing` package, limited to the first 100 results:

```graphql
query {
  environment(id: 834) {
    applied {
      exposures(
        filter: {
          uniqueIds: ["model.marketing.customers"] # Use this format for unique ID: RESOURCE_TYPE.PACKAGE_NAME.RESOURCE_NAME
        }, 
        first: 100
      ) {
        edges {
          node {
            accountId
            exposureType
            fqn
            projectId
            url
          }
        }
      }
    }
  }
}
```

### Fields[​](#fields "Direct link to Fields")

When querying for `exposures`, you can use the following fields:

# Fetching data...

### Key fields from nodes[​](#key-fields-from-nodes "Direct link to Key fields from nodes")

# Fetching data...

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
