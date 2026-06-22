# Lineage object schema

The lineage object allows you to query lineage across your resources.

The [Example query](#example-query) illustrates a few fields you can query with the `lineage` object. Refer to [Fields](#fields) to view the entire schema, which provides all possible fields you can query.

### Arguments[​](#arguments "Direct link to Arguments")

When querying for `lineage`, you can use the following arguments:

# Fetching data...

### Example query[​](#example-query "Direct link to Example query")

You can specify the `environmentId` and filter by "Model" as the resource type to see lineage information for all models in this environment, including their dependencies, materialization type, and metadata:

```graphql
query {
  environment(id: 834) {
    applied {
      lineage(
        filter: {"types": ["Model"]} # Return results for the Model type
      ) {
        name
        resourceType
        filePath
        projectId
        materializationType
        parentIds
        tags
        uniqueId
      }
    }
  }
}
```

### Fields[​](#fields "Direct link to Fields")

When querying for `lineage`, you can use the following fields:

# Fetching data...

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
