# Resources object schema

The resources object allows you to paginate across all resources in your environment.

The [Example query](#example-query) illustrates a few fields you can query with the `resources` object. Refer to [Fields](#fields) to view the entire schema, which provides all possible fields you can query.

### Arguments[​](#arguments "Direct link to Arguments")

When querying for `resources`, you can use the following arguments:

# Fetching data...

### Example query[​](#example-query "Direct link to Example query")

You can specify the `environmentId`, filter by "Model" as the type, and limit to the first 100 results to see comprehensive information about the first 100 model resources in this environment, including their metadata, tags, and file locations:

```graphql
query {
  environment(id: 834) {
    applied {
      resources(
        filter: {
          types: [
            Model
          ]
        }, 
        first: 100
      ) {
        edges {
          node {
            accountId
            description
            environmentId
            filePath
            meta
            name
            projectId
            resourceType
            uniqueId
            tags
          }
        }
      }
    }
  }
}
```

### Fields[​](#fields "Direct link to Fields")

When querying for `resources`, you can use the following fields:

# Fetching data...

### Key fields from nodes[​](#key-fields-from-nodes "Direct link to Key fields from nodes")

# Fetching data...

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
