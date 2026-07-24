# Tests object schema

dbt platform | Starter, Enterprise, Enterprise+ⓘ

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
    tests(first: 10, after: "{somePaginationCursorValue}") {
      runId
      accountId
      projectId
      uniqueId
      name
      columnName
      state
      paginationCursor
    }
  }
}
```

### Fields[​](#fields "Direct link to Fields")

When querying for `tests`, the following fields are available:

# Fetching data...
