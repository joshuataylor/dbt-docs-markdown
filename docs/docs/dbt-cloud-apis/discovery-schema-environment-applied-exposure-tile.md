# Exposure tile object schema

[Exposure health tiles](https://docs.getdbt.com/docs/explore/data-tile.md) distill data health signals for data consumers and can be embedded in downstream tools. You can query information on these tiles from the Discovery API.

The [Example query](#example-query) illustrates a few fields you can query with the `exposureTile` object. Refer to [Fields](#fields) to view the entire schema, which provides all possible fields you can query.

### Arguments[​](#arguments "Direct link to Arguments")

When querying for `exposureTile`, you can use the following arguments:

# Fetching data...

### Example query[​](#example-query "Direct link to Example query")

You can specify the `environmentId` and filter by a model's `uniqueId` to understand the data quality and metadata information for the exposure tile associated with the `customers` model in the `marketing` package:

```graphql
query {
  environment(id: 834) {
    applied {
      exposureTile(
        filter: {uniqueId: "model.marketing.customers"} # Use this format for unique ID: RESOURCE_TYPE.PACKAGE_NAME.RESOURCE_NAME
      ) {
        accountId # The account ID of this node
        environmentId
        projectId
        exposureType
        filePath
        quality
      }
    }
  }
}
```

### Fields[​](#fields "Direct link to Fields")

When querying for `exposureTile`, you can use the following fields:

# Fetching data...

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
