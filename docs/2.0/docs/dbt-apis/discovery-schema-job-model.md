# Model object schema

The model object allows you to query information about a particular model in a given job.

### Arguments[​](#arguments "Direct link to Arguments")

When querying for a `model`, the following arguments are available.

# Fetching data...

Below we show some illustrative example queries and outline the schema (all possible fields you can query) of the model object.

### Example query for finding parent models and sources[​](#example-query-for-finding-parent-models-and-sources "Direct link to Example query for finding parent models and sources")

The example query below uses the `parentsModels` and `parentsSources` fields to fetch information about a model’s parent models and parent sources. The jobID and uniqueID fields are placeholders that you will need to replace with your own values.

```graphql
{
  job(id: 123) {
    model(uniqueId: "model.jaffle_shop.dim_user") {
      parentsModels {
        runId
        uniqueId
        executionTime
      }
      parentsSources {
        runId
        uniqueId
        state
      }
    }
  }
}
```

### Example query for model timing[​](#example-query-for-model-timing "Direct link to Example query for model timing")

The example query below could be useful if you want to understand information around execution timing on a given model (start, end, completion).

```graphql
{
  job(id: 123) {
    model(uniqueId: "model.jaffle_shop.dim_user") {
      runId
      projectId
      name
      uniqueId
      resourceType
      executeStartedAt
      executeCompletedAt
      executionTime
    }
  }
}
```

### Example query for column-level information[​](#example-query-for-column-level-information "Direct link to Example query for column-level information")

You can use the following example query to understand more about the columns of a given model. This query will only work if the job has generated documentation; that is, it will work with the command `dbt docs generate`.

```graphql
{
  job(id: 123) {
    model(uniqueId: "model.jaffle_shop.dim_user") {
      columns {
        name
        index
        type
        comment
        description
        tags
        meta
      }
    }
  }
}
```

### Fields[​](#fields "Direct link to Fields")

When querying for a `model`, the following fields are available:

# Fetching data...

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
