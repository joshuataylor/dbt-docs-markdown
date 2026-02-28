# Tests object schema

[Tests](https://docs.getdbt.com/docs/build/data-tests.md) are assertions you make about your models and other resources in your dbt project. When you run `dbt test`, dbt will tell you if each test in your project passes or fails. You can query tests through the Discovery API to understand information about them.

The [Example query](#example-query) illustrates a few fields you can query with the `tests` object. Refer to [Fields](#fields) to view the entire schema, which provides all possible fields you can query.

### Arguments[​](#arguments "Direct link to Arguments")

When querying for `tests`, you can use the following arguments:

# Fetching data...

### Example query[​](#example-query "Direct link to Example query")

You can use the `environmentId` and filter by test types to return metadata for all tests in the environment:

```graphql
query {
  environment(id: 834) {
    applied {
      tests(
        filter: {
          testTypes: [
            GENERIC_DATA_TEST,
            SINGULAR_DATA_TEST,
            UNIT_TEST
          ]
        }, 
        first: 100
      ) {
        edges {
          node {
            name
            model
            description
            expect
            resourceType
            testType
            given
          }
        }
      }
    }
  }
}
```

### Fields[​](#fields "Direct link to Fields")

When querying for `tests`, you can use the following fields:

# Fetching data...

### Key fields from nodes[​](#key-fields-from-nodes "Direct link to Key fields from nodes")

# Fetching data...

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
