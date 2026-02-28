# Seeds object schema

[Seeds](https://docs.getdbt.com/docs/build/seeds.md) are CSV files in your dbt project that dbt can load into your data warehouse. You can query seeds through the Discovery API.

The [Example query](#example-query) illustrates a few fields you can query with the `seeds` object. Refer to [Fields](#fields) to view the entire schema, which provides all possible fields you can query.

### Arguments[​](#arguments "Direct link to Arguments")

When querying for `seeds`, you can use the following arguments:

# Fetching data...

### Example query[​](#example-query "Direct link to Example query")

You can specify the `environmentId`, filter by the database, and limit to the first 100 to show information about the first 100 seed files in the `analytics` database, including their metadata and file locations:

```graphql
query ($environmentId: BigInt!, $first: Int!, $filter: GenericMaterializedFilter) {
  environment(id: $environmentId) {
    applied {
      seeds(
        first: 100,
        filter: {
          database: "analytics"
        }
      ) {
        edges {
          node {
            description
            name
            filePath
            projectId
            fqn
            tags
            uniqueId
            resourceType
          }
        }
      }
    }
  }
}
```

### Fields[​](#fields "Direct link to Fields")

When querying for `seeds`, you can use the following fields:

# Fetching data...

### Key fields from nodes[​](#key-fields-from-nodes "Direct link to Key fields from nodes")

# Fetching data...

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
