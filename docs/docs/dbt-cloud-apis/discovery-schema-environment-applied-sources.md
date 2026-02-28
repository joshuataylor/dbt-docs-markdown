# Sources object schema

[Sources](https://docs.getdbt.com/docs/build/sources.md) make it possible to name and describe the data loaded into your warehouse by your extract and load tools. You can query sources through the Discovery API.

The [Example query](#example-query) illustrates a few fields you can query with the `sources` object. Refer to [Fields](#fields) to view the entire schema, which provides all possible fields you can query.

### Arguments[​](#arguments "Direct link to Arguments")

When querying for `sources`, you can use the following arguments:

# Fetching data...

### Example query[​](#example-query "Direct link to Example query")

You can specify the `environmentId` and filter on the database name, to return the freshness and execution status, for the first 100 sources from the given database:

```graphql
query {
  environment(id: 834) {
    applied {
      sources(
        filter: {
          database: "analytics"
        }, 
        first: 100
      ) {
        edges {
          node {
            name
            fqn
            description
            filePath
            freshness {
              freshnessChecked
              freshnessStatus
            }
            sourceName
            sourceDescription
            tests {
              name
              description
              testType
              executionInfo {
                lastRunStatus
              }
            }
          }
        }
      }
    }
  }
}
```

### Fields[​](#fields "Direct link to Fields")

When querying for `sources`, you can use the following fields:

# Fetching data...

### Key fields from nodes[​](#key-fields-from-nodes "Direct link to Key fields from nodes")

# Fetching data...

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
