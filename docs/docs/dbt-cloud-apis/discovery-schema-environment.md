# Environment object schema

You can use the environment object to query and compare definition (intended) and applied (actual) states for nodes (models, seeds, snapshots, models, and more) in your dbt project. For example, you specify an `environmentId` to learn more about a particular model (or other node type) in that environment.

The [Example queries](#example-queries) illustrate a few fields you can query with this `environment` object. Refer to [Fields](#fields) to view the entire schema, which provides all possible fields you can query.

### Arguments[​](#arguments "Direct link to Arguments")

When querying for `environment`, you can use the following arguments.

# Fetching data...

### Example queries[​](#example-queries "Direct link to Example queries")

You can use your production environment's `id`:

```graphql
query Example {
	environment(id: 834){ # Get the latest state of the production environment
		applied { # The state of an executed node as it exists as an object in the database
			models(first: 100){ # Pagination to ensure manageable response for large projects
				edges { node {
					uniqueId, name, description, rawCode, compiledCode, # Basic properties
					database, schema, alias, # Table/view identifier (can also filter by)
					executionInfo {executeCompletedAt, executionTime}, # Metadata from when the model was built
					tests {name, executionInfo{lastRunStatus, lastRunError}}, # Latest test results
					catalog {columns {name, description, type}, stats {label, value}}, # Catalog info
					ancestors(types:[Source]) {name, ...on SourceAppliedStateNode {freshness{maxLoadedAt, freshnessStatus}}}, # Source freshness }
					children {name, resourceType}}} # Immediate dependencies in lineage
				totalCount } # Number of models in the project
		}
		definition { # The logical state of a given project node given its most recent manifest generated
			models(first: 100, filter:{access:public}){ # Filter on model access (or other properties)
				edges { node {
					rawCode, # Compare to see if/how the model has changed since the last build
					jobDefinitionId, runGeneratedAt,	# When the code was last compiled or run
					contractEnforced, group, version}}} # Model governance
		}
	}
```

With the deprecation of the data type `Int` for `id`, below is an example of replacing it with `BigInt`:

```graphql
query ($environmentId: BigInt!, $first: Int!) {
  environment(id: $environmentId) {
    applied {
      models(first: $first) {
        edges {
          node {
            uniqueId
            executionInfo {
              lastRunId
            }
          }
        }
      }
    }
  }
}
```

With the deprecation of `modelByEnvironment`, below is an example of replacing it with `environment`:

```graphql
query ($environmentId: BigInt!, $uniqueId: String) {
  environment(id: $environmentId) {
    applied {
      modelHistoricalRuns(uniqueId: $uniqueId) {
        uniqueId
        executionTime
        executeCompletedAt
      }
    }
  }
}
```

### Fields[​](#fields "Direct link to Fields")

When querying an `environment`, you can use the following fields.

# Fetching data...

For details on querying the `applied` field of `environment`, you can visit: [Applied](https://docs.getdbt.com/docs/dbt-cloud-apis/discovery-schema-environment-applied.md)

For details querying the `definition` field of `environment`, you can visit: [Definition](https://docs.getdbt.com/docs/dbt-cloud-apis/discovery-schema-environment-definition.md)

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
