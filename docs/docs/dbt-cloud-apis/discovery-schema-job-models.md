# Models object schema

The models object allows you to query information about all models in a given job.

### Arguments[​](#arguments "Direct link to Arguments")

When querying for `models`, the following arguments are available.

# Fetching data...

Below we show some illustrative example queries and outline the schema of the models object.

### Example queries[​](#example-queries "Direct link to Example queries")

The database, schema, and identifier arguments are all optional. This means that with this endpoint you can:

* Find a specific model by providing `<database>.<schema>.<identifier>`
* Find all of the models in a database and/or schema by providing `<database>` and/or `<schema>`

#### Find models by their database, schema, and identifier[​](#find-models-by-their-database-schema-and-identifier "Direct link to Find models by their database, schema, and identifier")

The example query below finds a model by its unique database, schema, and identifier.

```graphql
{
  job(id: 123) {
    models(database:"analytics", schema: "analytics", identifier:"dim_customers") {
      uniqueId
    }
  }
}
```

#### Find models by their schema[​](#find-models-by-their-schema "Direct link to Find models by their schema")

The example query below finds all models in this schema and their respective execution times.

```graphql
{
  job(id: 123) {
    models(schema: "analytics") {
      uniqueId
      executionTime
    }
  }
}
```

### Fields[​](#fields "Direct link to Fields")

The models object can access the *same fields* as the [Model node](https://docs.getdbt.com/docs/dbt-cloud-apis/discovery-schema-job-model.md). The difference is that the models object can output a list so instead of querying for fields for one specific model, you can query for those parameters for all models within a jobID, database, and so on.

When querying for `models`, the following fields are available:

# Fetching data...

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
