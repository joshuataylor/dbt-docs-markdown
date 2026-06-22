# Exposure object schema

The exposure object allows you to query information about a particular exposure. To learn more, refer to [Add Exposures to your DAG](https://docs.getdbt.com/docs/build/exposures.md).

### Arguments[​](#arguments "Direct link to Arguments")

When querying for an `exposure`, the following arguments are available.

# Fetching data...

Below we show some illustrative example queries and outline the schema of the exposure object.

### Example query[​](#example-query "Direct link to Example query")

The example below queries information about an exposure including the owner's name and email, the URL, and information about parent sources and parent models.

```graphql
{
  job(id: 123) {
    exposure(name: "my_awesome_exposure") {
      runId
      projectId
      name
      uniqueId
      resourceType
      ownerName
      url
      ownerEmail
      parentsSources {
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
      parentsModels {
        uniqueId
      }
    }
  }
}
```

### Fields[​](#fields "Direct link to Fields")

When querying for an `exposure`, the following fields are available:

# Fetching data...
