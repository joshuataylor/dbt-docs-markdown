# Source object schema

dbt platform | Starter, Enterprise, Enterprise+ⓘ

The source object allows you to query information about a particular source in a given job.

### Arguments[​](#arguments "Direct link to Arguments")

When querying for a `source`, the following arguments are available.

# Fetching data...

Below we show some illustrative example queries and outline the schema of the source object.

### Example query[​](#example-query "Direct link to Example query")

The query below pulls relevant information about a given source. For instance, you can view the load time and the state (pass, fail, error) of that source.

```graphql
{
  job(id: 123) {
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

When querying for a `source`, the following fields are available:

# Fetching data...
