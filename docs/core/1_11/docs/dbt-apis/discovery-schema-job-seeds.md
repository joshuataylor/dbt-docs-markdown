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
    seeds(first: 10, after: "{somePaginationCursorValue}") {
      uniqueId
      name
      executionTime
      status
      paginationCursor
    }
  }
}
```

### Fields[​](#fields "Direct link to Fields")

When querying for `seeds`, the following fields are available:

# Fetching data...
