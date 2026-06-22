# Tags object schema

[Tags](https://docs.getdbt.com/reference/resource-configs/tags.md) provide a mechanism to categorize and group resources within a dbt project, enabling selective execution and management of these resources. You can query tags through the Discovery API.

The [Example query](#example-query) illustrates a few fields you can query with the `tags` object. Refer to [Fields](#fields) to view the entire schema, which provides all possible fields you can query.

### Example query[​](#example-query "Direct link to Example query")

You can use the `environmentId` to return the name of all the tags in your environment:

```graphql
query {
  environment(id: 834) {
    applied {
      tags {
        name
      }
    }
  }
}
```

### Fields[​](#fields "Direct link to Fields")

When querying for `tags`, you can use the following fields:

# Fetching data...

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
