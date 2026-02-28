# Tests object schema

The tests object allows you to query information about all tests in a given job.

### Arguments[​](#arguments "Direct link to Arguments")

When querying for `tests`, the following arguments are available.

# Fetching data...

Below we show some illustrative example queries and outline the schema (all possible fields you can query) of the tests object.

### Example query[​](#example-query "Direct link to Example query")

The example query below finds all tests in this job and includes information about those tests.

```graphql
{
  job(id: 123) {
    tests {
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

When querying for `tests`, the following fields are available:

# Fetching data...

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
