# Exposures object schema

The exposures object allows you to query information about all exposures in a given job. To learn more, refer to [Add Exposures to your DAG](https://docs.getdbt.com/docs/build/exposures.md).

### Arguments[​](#arguments "Direct link to Arguments")

When querying for `exposures`, the following arguments are available.

# Fetching data...

Below we show some illustrative example queries and outline the schema of the exposures object.

### Example query[​](#example-query "Direct link to Example query")

The example below queries information about all exposures in a given job including the owner's name and email, the URL, and information about parent sources and parent models for each exposure.

```graphql
{
  job(id: 123) {
    exposures(jobId: 123) {
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

When querying for `exposures`, the following fields are available:

# Fetching data...

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
