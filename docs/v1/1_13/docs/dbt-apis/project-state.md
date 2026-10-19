# Project state in dbt

dbt provides a stateful way of deploying dbt. Artifacts are accessible programmatically via the [Discovery API](./discovery-querying.md) in the metadata platform.

With the implementation of the `environment` endpoint in the Discovery API, we've introduced the idea of multiple states. The Discovery API provides a single API endpoint that returns the latest state of models, sources, and other nodes in the DAG.

A single [deployment environment](../environments-in-dbt.md) should represent the production state of a given dbt project.

There are two states that can be queried in dbt:

* **Applied state** refers to what exists in the data warehouse after a successful `dbt run`. The model build succeeds and now exists as a table in the warehouse.

* **Definition state** depends on what exists in the project given the code defined in it (for example, manifest state), which hasn’t necessarily been executed in the data platform (maybe just the result of `dbt compile`).

## Definition (logical) vs. applied state of dbt nodes

In a dbt project, the state of a node *definition* represents the configuration, transformations, and dependencies defined in the SQL and YAML files. It captures how the node should be processed in relation to other nodes and tables in the data warehouse and may be produced by a `dbt build`, `run`, `parse`, or `compile`. It changes whenever the project code changes.

A node’s *applied state* refers to the node’s actual state after it has been successfully executed in the DAG; for example, models are executed; thus, their state is applied to the data warehouse via `dbt run` or `dbt build`. It changes whenever a node is executed. This state represents the result of the transformations and the actual data stored in the database, which for models can be a table or a view based on the defined logic.

The applied state includes execution info, which contains metadata about how the node arrived in the applied state. The fields within `executionInfo` track two related but distinct concepts:

| Concept                                | Description                                                                                                                                                                    | Fields                                                                                                                        |
| -------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ----------------------------------------------------------------------------------------------------------------------------- |
| Most recent run attempt                | The latest run regardless of outcome (success, error, or skip)                                                                                                                 | `lastRunId`, `lastRunStatus`, `lastRunError`, `lastRunGeneratedAt`, `lastJobDefinitionId`                                     |
| Most recent successful materialization | The last run in which the node was built in the data warehouse.<br />When a run errors out, the node isn't rebuilt, so these fields remain pinned to the prior successful run. | `executeStartedAt`, `executeCompletedAt`, `executionTime`, `runGeneratedAt`, `lastSuccessRunId`, `lastSuccessJobDefinitionId` |

For example, if a model's most recent run errors out, `lastRunStatus` will be `error` and `lastRunGeneratedAt` will reference that failed run, while `executeCompletedAt` and `lastSuccessRunId` will still reference the prior run in which the model was successfully materialized.

Here’s how you can query and compare the definition vs. applied state of a model using the Discovery API:

```graphql
query Compare($environmentId: Int!, $first: Int!) {
	environment(id: $environmentId) {
		definition {
			models(first: $first) {
				edges {
					node {
						name
						rawCode
					}
				}
			}
		}
		applied {
			models(first: $first) {
				edges {
					node {
						name
						rawCode 
						executionInfo {
							executeCompletedAt
						}
					}
				}
			}
		}
	}
}
```

Most Discovery API use cases will favor the *applied state* since it pertains to what has actually been run and can be analyzed.

## Affected states by node type

The following table shows the states of dbt nodes and how they are affected by the Discovery API.

| Node                                                                                   | Executed in DAG | Created by execution | Exists in database | Lineage               | States               |
| -------------------------------------------------------------------------------------- | --------------- | -------------------- | ------------------ | --------------------- | -------------------- |
| [Analysis](../build/analyses.md)                             | No              | No                   | No                 | Upstream              | Definition           |
| [Data test](../build/data-tests.md)                          | Yes             | Yes                  | No                 | Upstream              | Applied & definition |
| [Exposure](../build/exposures.md)                            | No              | No                   | No                 | Upstream              | Definition           |
| [Group](../build/groups.md)                                  | No              | No                   | No                 | Downstream            | Definition           |
| [Macro](../build/jinja-macros.md)                            | Yes             | No                   | No                 | N/A                   | Definition           |
| [Metric](../build/metrics-overview.md)                       | No              | No                   | No                 | Upstream & downstream | Definition           |
| [Model](../build/models.md)                                  | Yes             | Yes                  | Yes                | Upstream & downstream | Applied & definition |
| [Saved queries](../build/saved-queries.md)<br />(not in API) | N/A             | N/A                  | N/A                | N/A                   | N/A                  |
| [Seed](../build/seeds.md)                                    | Yes             | Yes                  | Yes                | Downstream            | Applied & definition |
| [Semantic model](../build/semantic-models.md)                | No              | No                   | No                 | Upstream & downstream | Definition           |
| [Snapshot](../build/snapshots.md)                            | Yes             | Yes                  | Yes                | Upstream & downstream | Applied & definition |
| [Source](../build/sources.md)                                | Yes             | No                   | Yes                | Downstream            | Applied & definition |
| [Unit tests](../build/unit-tests.md)                         | Yes             | Yes                  | No                 | Downstream            | Definition           |

## Caveats about state/metadata updates

Over time, Cloud Artifacts will provide information to maintain state for features/services in dbt and enable you to access state in dbt and its downstream ecosystem. Cloud Artifacts is currently focused on the latest production state, but this focus will evolve.

Here are some limitations of the state representation in the Discovery API:

* Users must access the default production environment to know the latest state of a project.
* The API gets the definition from the latest manifest generated in a given deployment environment, but that often won’t reflect the latest project code state.
* Compiled code results may be outdated depending on dbt run step order and failures.
* Catalog info can be outdated, or incomplete (in the applied state), based on if/when `docs generate` was last run.
* Source freshness checks can be out of date (in the applied state) depending on when the command was last run, and it’s not included in `build`.
