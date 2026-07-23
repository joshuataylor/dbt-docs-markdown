# Model Historical Runs object schema

The model historical runs object allows you to query information about a model's run history.

The [Example query](#example-query) illustrates a few fields you can query with the `modelHistoricalRuns` object. Refer to [Fields](#fields) to view the entire schema, which provides all possible fields you can query.

### Arguments[​](#arguments "Direct link to Arguments")

When querying for `modelHistoricalRuns`, you can use the following arguments:

# Fetching data...

### Example query[​](#example-query "Direct link to Example query")

You can specify the `environmentId` and the model's `uniqueId` to return the model and its execution history for the `customers` model in the `marketing` package, including performance metrics and test results for the last 20 times it was run, regardless of which job ran it.

```graphql
query {
  environment(id: 834) {
    applied {
      modelHistoricalRuns(
        uniqueId: "model.marketing.customers" # Use this format for unique ID: RESOURCE_TYPE.PACKAGE_NAME.RESOURCE_NAME
        lastRunCount: 20
      ) {
        runId # Get historical results for a particular model
        runGeneratedAt
        executionTime # View build time across runs
        status
        tests {
          name
          status
          executeCompletedAt
        } # View test results across runs
      }
    }
  }
}
```

### Fields[​](#fields "Direct link to Fields")

When querying for `modelHistoricalRuns`, you can use the following fields:

# Fetching data...
