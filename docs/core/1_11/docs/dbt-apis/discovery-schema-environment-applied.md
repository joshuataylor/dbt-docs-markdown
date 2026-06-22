# Applied object schema

The applied object allows you to query information about a particular model based on `environmentId`.

The [Example queries](#example/docs/dbt-apis-queries) illustrate a few fields you can query with this `environment` object. Refer to [Fields](#fields) to view the entire schema, which provides all possible fields you can query.

`executionInfo` fields track two different concepts

Within `executionInfo`, `lastRun*` fields reflect the most recent run *attempt* (regardless of outcome), while `execute*`, `executionTime`, `runGeneratedAt`, and `lastSuccess*` fields reflect the most recent *successful* materialization. Refer to [Project state](https://docs.getdbt.com/docs/dbt-apis/project-state.md#definition-logical-vs-applied-state-of-dbt-nodes) for more information.

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
					ancestors(types:[Source]) {name, ...on SourceAppliedStateNestedNode {freshness{maxLoadedAt, freshnessStatus}}}, # Source freshness }
					children {name, resourceType}}} # Immediate dependencies in lineage
				totalCount } # Number of models in the project
		}
	}
}
```

### Fields[​](#fields "Direct link to Fields")

When querying the `applied` field of `environment`, you can use the following fields.

# Fetching data...

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
