# About the Discovery API schema

With the Discovery API, you can query the metadata in dbt to learn more about your dbt deployments and the data they generate. You can analyze the data to make improvements. If you are new to the API, refer to [About the Discovery API](https://docs.getdbt.com/docs/dbt-cloud-apis/discovery-api.md) for an introduction. You might also find the [use cases and examples](https://docs.getdbt.com/docs/dbt-cloud-apis/discovery-use-cases-and-examples.md) helpful.

The Discovery API *schema* provides all the pieces necessary to query and interact with the Discovery API. The most common queries use the `environment` endpoint:

[![](/img/icons/dbt-bit.svg)](https://docs.getdbt.com/docs/dbt-cloud-apis/discovery-schema-environment.md)

#### [Environment schema](https://docs.getdbt.com/docs/dbt-cloud-apis/discovery-schema-environment.md)

[Query and compare a model’s definition (intended) and its applied (actual) state.](https://docs.getdbt.com/docs/dbt-cloud-apis/discovery-schema-environment.md)

[![](/img/icons/dbt-bit.svg)](https://docs.getdbt.com/docs/dbt-cloud-apis/discovery-schema-environment-applied.md)

#### [Applied schema](https://docs.getdbt.com/docs/dbt-cloud-apis/discovery-schema-environment-applied.md)

[Query the actual state of objects and metadata in the warehouse after a \`dbt run\` or \`dbt build\`.](https://docs.getdbt.com/docs/dbt-cloud-apis/discovery-schema-environment-applied.md)

[![](/img/icons/dbt-bit.svg)](https://docs.getdbt.com/docs/dbt-cloud-apis/discovery-schema-environment-definition.md)

#### [Definition schema](https://docs.getdbt.com/docs/dbt-cloud-apis/discovery-schema-environment-definition.md)

[Query intended state in project code and configuration defined in your dbt project.](https://docs.getdbt.com/docs/dbt-cloud-apis/discovery-schema-environment-definition.md)

[![](/img/icons/dbt-bit.svg)](https://docs.getdbt.com/docs/dbt-cloud-apis/discovery-schema-environment-applied-modelHistoricalRuns.md)

#### [Model Historical Runs schema](https://docs.getdbt.com/docs/dbt-cloud-apis/discovery-schema-environment-applied-modelHistoricalRuns.md)

[Query information about a model's run history.](https://docs.getdbt.com/docs/dbt-cloud-apis/discovery-schema-environment-applied-modelHistoricalRuns.md)

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
