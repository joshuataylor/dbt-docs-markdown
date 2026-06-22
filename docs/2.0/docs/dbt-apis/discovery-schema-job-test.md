# Test object schema

The test object allows you to query information about a particular test.

### Arguments[​](#arguments "Direct link to Arguments")

When querying for a `test`, the following arguments are available.

# Fetching data...

Below we show some illustrative example queries and outline the schema (all possible fields you can query) of the test object.

### Example query[​](#example-query "Direct link to Example query")

The example query below outputs information about a test including the state of the test result. In order of severity, the result can be one of these: "error", "fail", "warn", or "pass".

```graphql
{
  job(id: 123) {
    test(uniqueId: "test.internal_analytics.not_null_metrics_id") {
      runId
      accountId
      projectId
      uniqueId
      name
      columnName
      state
    }
  }
}
```

### Fields[​](#fields "Direct link to Fields")

When querying for a `test`, the following fields are available:

# Fetching data...

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
