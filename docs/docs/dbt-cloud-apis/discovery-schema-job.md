# Job object schema

The job object allows you to query information about a particular model based on `jobId` and, optionally, a `runId`.

If you don't provide a `runId`, the API returns information on the latest runId of a job.

The [example query](#example-query) illustrates a few fields you can query in this `job` object. Refer to [Fields](#fields) to see the entire schema, which provides all possible fields you can query.

### Arguments[​](#arguments "Direct link to Arguments")

When querying for `job`, you can use the following arguments.

# Fetching data...

### Example Query[​](#example-query "Direct link to Example Query")

You can use your production job's `id`.

```graphql
query JobQueryExample {
  # Provide runId for looking at specific run, otherwise it defaults to latest run
  job(id: 940) {
    # Get all models from this job's latest run
    models(schema: "analytics") {
      uniqueId
      executionTime
    }

    # Or query a single node
    source(uniqueId: "source.jaffle_shop.snowplow.event") {
      uniqueId
      sourceName
      name
      state
      maxLoadedAt
      criteria {
        warnAfter {
          period
          count
        }
        errorAfter {
          period
          count
        }
      }
      maxLoadedAtTimeAgoInS
    }
  }
}
```

### Fields[​](#fields "Direct link to Fields")

When querying an `job`, you can use the following fields.

# Fetching data...

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
