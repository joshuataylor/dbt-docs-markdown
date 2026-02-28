# Snapshots object schema

The snapshots object allows you to query information about all snapshots in a given job.

### Arguments[​](#arguments "Direct link to Arguments")

When querying for `snapshots`, the following arguments are available.

# Fetching data...

Below we show some illustrative example queries and outline the schema of the snapshots object.

### Example query[​](#example-query "Direct link to Example query")

The database, schema, and identifier arguments are optional. This means that with this endpoint you can:

* Find a specific snapshot by providing `<database>.<schema>.<identifier>`
* Find all of the snapshots in a database and/or schema by providing `<database>` and/or `<schema>`

#### Find snapshots information for a job[​](#find-snapshots-information-for-a-job "Direct link to Find snapshots information for a job")

The example query returns information about all snapshots in this job.

```graphql
{
  job(id: 123) {
    snapshots {
      uniqueId
      name
      executionTime
      environmentId
      executeStartedAt
      executeCompletedAt
    }
  }
}
```

### Fields[​](#fields "Direct link to Fields")

When querying for `snapshots`, the following fields are available:

# Fetching data...

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
