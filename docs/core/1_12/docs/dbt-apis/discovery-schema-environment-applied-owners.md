# Owners object schema

[Owners](https://docs.getdbt.com/docs/build/groups.md) help you identify the user or domain responsible for a dbt asset. For most assets, owners are defined in your project code using groups. Exposures are an exception: for downstream exposures that represent BI assets, owners are automatically pulled from the downstream tool based on who owns that asset. You can query ownership information through the Discovery API.

The [Example query](#example-query) illustrates a few fields you can query with the `owners` object. Refer to [Fields](#fields) to view the entire schema, which provides all possible fields you can query.

### Arguments[​](#arguments "Direct link to Arguments")

When querying for `owners`, you can use the following arguments:

# Fetching data...

### Example query[​](#example-query "Direct link to Example query")

You can specify the `environmentId` and "exposure" as the `OwnerResourceType` to return people who own exposures (downstream BI assets) in this environment, including their contact information.

```graphql
query {
  environment(id: 834) {
    applied {
      owners(resource: exposure) {
        email
        name
      }
    }
  }
}
```

### Fields[​](#fields "Direct link to Fields")

When querying for `owners`, you can use the following fields:

# Fetching data...

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
