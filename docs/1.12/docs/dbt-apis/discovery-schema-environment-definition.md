# Definition object schema

The definition object allows you to query the logical state of a given project node given its most recent manifest generated models.

The [Example queries](#example-queries) illustrate a few fields you can query with this `definition` object. Refer to [Fields](#fields) to view the entire schema, which provides all possible fields you can query.

### Example queries[​](#example-queries "Direct link to Example queries")

You can use your production environment's `id`:

```graphql
query Example {
	environment(id: 834){ # Get the latest state of the production environment
		definition { # The logical state of a given project node given its most recent manifest generated
			models(first: 100, filter:{access:public}){ # Filter on model access (or other properties)
				edges { node {
					rawCode, # Compare to see if/how the model has changed since the last build
					jobDefinitionId, runGeneratedAt,	# When the code was last compiled or run
					contractEnforced, group, version}}} # Model governance
		}
	}
}
```

### Fields[​](#fields "Direct link to Fields")

When querying the `definition` field of `environment`, you can use the following fields.

# Fetching data...

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
