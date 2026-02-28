# Seeds object schema

The seeds object allows you to query information about all seeds in a given job.

### Arguments[​](#arguments "Direct link to Arguments")

When querying for `seeds`, the following arguments are available.

# Fetching data...

Below we show some illustrative example queries and outline the schema of the seeds object.

### Example query[​](#example-query "Direct link to Example query")

The example query below pulls relevant information about all seeds in a given job. For instance, you can view load times.

```graphql
{
  job(id: 123) {
    seeds {
      uniqueId
      name
      executionTime
      status
    }
  }
}
```

### Fields[​](#fields "Direct link to Fields")

When querying for `seeds`, the following fields are available:

# Fetching data...

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
