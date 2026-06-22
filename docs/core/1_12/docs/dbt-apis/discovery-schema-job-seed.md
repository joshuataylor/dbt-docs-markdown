# Seed object schema

The seed object allows you to query information about a particular seed in a given job.

### Arguments[​](#arguments "Direct link to Arguments")

When querying for a `seed`, the following arguments are available.

# Fetching data...

Below we show some illustrative example queries and outline the schema of the seed object.

### Example query[​](#example-query "Direct link to Example query")

The example query below pulls relevant information about a given seed. For instance, you can view the load time.

```graphql
{
  job(id: 123) {
    seed(uniqueId: "seed.jaffle_shop.raw_customers") {
      database
      schema
      uniqueId
      name
      status
      error
    }
  }
}
```

### Fields[​](#fields "Direct link to Fields")

When querying for a `seed`, the following fields are available:

# Fetching data...

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
