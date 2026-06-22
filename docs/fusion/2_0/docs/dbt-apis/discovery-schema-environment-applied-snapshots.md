# Snapshots object schema

[Snapshots](https://docs.getdbt.com/docs/build/snapshots.md) represent point-in-time copies of your data, allowing you to track historical changes. You can query your snapshots from the Discovery API.

The [Example query](#example-query) illustrates a few fields you can query with the `snapshots` object. Refer to [Fields](#fields) to view the entire schema, which provides all possible fields you can query.

### Arguments[​](#arguments "Direct link to Arguments")

When querying for `snapshots`, you can use the following arguments:

# Fetching data...

### Example query[​](#example-query "Direct link to Example query")

You can specify the `environmentId`, filter by the database, and limit to the first 100 to see the first 100 snapshots in the `analytics` database, including their execution performance and status information:

```graphql
query {
  environment(id: 834) {
    applied {
      snapshots(
        filter: {
          database: "analytics"
        }, 
        first: 100
      ) {
        edges {
          node {
            executionInfo {
              compileCompletedAt
              compileStartedAt
              executeCompletedAt
              executeStartedAt
              executionTime
              lastRunStatus
              lastRunId
            }
            fqn
            name
          }
        }
      }
    }
  }
}
```

### Fields[​](#fields "Direct link to Fields")

When querying for `snapshots`, you can use the following fields:

# Fetching data...

### Key fields from nodes[​](#key-fields-from-nodes "Direct link to Key fields from nodes")

# Fetching data...

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
