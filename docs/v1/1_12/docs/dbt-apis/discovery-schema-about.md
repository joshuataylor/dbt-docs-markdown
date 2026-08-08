# About the Discovery API schema

dbt platform | Starter, Enterprise, Enterprise+ⓘ

With the Discovery API, you can query the metadata in dbt to learn more about your dbt deployments and the data they generate. You can analyze the data to make improvements. If you are new to the API, refer to [About the Discovery API](./discovery-api.md) for an introduction. You might also find the [use cases and examples](./discovery-use-cases-and-examples.md) helpful.

The Discovery API *schema* provides all the pieces necessary to query and interact with the Discovery API. The most common queries use the `environment` endpoint:

[![](/img/icons/dbt-bit.svg)](./discovery-schema-environment.md)

#### [Environment schema](./discovery-schema-environment.md)

[Query and compare a model’s definition (intended) and its applied (actual) state.](./discovery-schema-environment.md)

[![](/img/icons/dbt-bit.svg)](./discovery-schema-environment-applied.md)

#### [Applied schema](./discovery-schema-environment-applied.md)

[Query the actual state of objects and metadata in the warehouse after a \`dbt run\` or \`dbt build\`.](./discovery-schema-environment-applied.md)

[![](/img/icons/dbt-bit.svg)](./discovery-schema-environment-definition.md)

#### [Definition schema](./discovery-schema-environment-definition.md)

[Query intended state in project code and configuration defined in your dbt project.](./discovery-schema-environment-definition.md)

[![](/img/icons/dbt-bit.svg)](./discovery-schema-environment-applied-modelHistoricalRuns.md)

#### [Model Historical Runs schema](./discovery-schema-environment-applied-modelHistoricalRuns.md)

[Query information about a model's run history.](./discovery-schema-environment-applied-modelHistoricalRuns.md)
