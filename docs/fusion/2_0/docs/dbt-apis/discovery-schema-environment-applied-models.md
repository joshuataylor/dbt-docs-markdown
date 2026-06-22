# Models object schema

[Models](https://docs.getdbt.com/docs/build/models.md) are the foundational dbt resource that transform raw data into curated datasets using SQL (or Python). Each model represents a single SELECT statement, typically materialized as a table or view in your warehouse. You can query information about models through the Discovery API.

The [Example query](#example-query) illustrates a few fields you can query with the `models` object. Refer to [Fields](#fields) to view the entire schema, which provides all possible fields you can query.

### Arguments[​](#arguments "Direct link to Arguments")

When querying for `models`, you can use the following arguments:

# Fetching data...

### Example query[​](#example-query "Direct link to Example query")

You can specify the `environmentId` to return model information for all models in the given environment, including their metadata, configuration, tests, and ownership details, limited to the first 100 results:

```graphql
query {
  environment(id: 834) {
    applied {
      models (first: 100) {
        edges {
          node {
            name
            description
            access
            accountId
            catalog {
              owner
            }
            config
            environmentId
            tests {
              name
              description
            }
          }
        }
      }
    }
  }
}
```

### Fields[​](#fields "Direct link to Fields")

When querying for `models`, you can use the following fields:

# Fetching data...

### Key fields from nodes[​](#key-fields-from-nodes "Direct link to Key fields from nodes")

# Fetching data...

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
