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
    snapshots(first: 10, after: "{somePaginationCursorValue}") {
      uniqueId
      name
      executionTime
      environmentId
      executeStartedAt
      executeCompletedAt
      paginationCursor
    }
  }
}
```

### Fields[​](#fields "Direct link to Fields")

When querying for `snapshots`, the following fields are available:

# Fetching data...
