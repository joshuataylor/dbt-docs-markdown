# Packages object schema

[dbt packages](https://docs.getdbt.com/docs/build/packages.md) are libraries with models, macros, and other resources that tackle a specific problem area utilized by dbt projects. You can query project packages through the Discovery API.

The [Example query](#example-query) illustrates a few fields you can query with the `packages` object. Refer to [Fields](#fields) to view the entire schema, which provides all possible fields you can query.

### Arguments[​](#arguments "Direct link to Arguments")

When querying for `packages`, you can use the following arguments:

# Fetching data...

### Example query[​](#example-query "Direct link to Example query")

You can specify the `environmentId` and "model" as the resource to see all dbt packages in this environment that contain model resources:

```graphql
query {
  environment(id: 834) {
    applied {
      packages(resource: "model")
    }
  }
}
```

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
