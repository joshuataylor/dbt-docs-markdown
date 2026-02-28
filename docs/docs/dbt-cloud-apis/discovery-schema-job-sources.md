# Sources object schema

The sources object allows you to query information about all sources in a given job.

### Arguments[​](#arguments "Direct link to Arguments")

When querying for `sources`, the following arguments are available.

# Fetching data...

Below we show some illustrative example queries and outline the schema of the sources object.

### Example queries[​](#example-queries "Direct link to Example queries")

The database, schema, and identifier arguments are optional. This means that with this endpoint you can:

* Find a specific source by providing `<database>.<schema>.<identifier>`
* Find all of the sources in a database and/or schema by providing `<database>` and/or `<schema>`

#### Finding sources by their database, schema, and identifier[​](#finding-sources-by-their-database-schema-and-identifier "Direct link to Finding sources by their database, schema, and identifier")

The example query below finds a source by its unique database, schema, and identifier.

```graphql
{
  job(id: 123) {
    sources(
      database: "analytics"
      schema: "analytics"
      identifier: "dim_customers"
    ) {
      uniqueId
    }
  }
}
```

#### Finding sources by their schema[​](#finding-sources-by-their-schema "Direct link to Finding sources by their schema")

The example query below finds all sources in this schema and their respective states (pass, error, fail).

```graphql
{
  job(id: 123) {
    sources(schema: "analytics") {
      uniqueId
      state
    }
  }
}
```

### Fields[​](#fields "Direct link to Fields")

The sources object can access the *same fields* as the [source node](https://docs.getdbt.com/docs/dbt-cloud-apis/discovery-schema-job-source.md). The difference is that the sources object can output a list so instead of querying for fields for one specific source, you can query for those parameters for all sources within a jobID, database, and so on.

When querying for `sources`, the following fields are available:

# Fetching data...

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
