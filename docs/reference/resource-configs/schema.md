# schema

* Model
* Seeds
* Snapshots
* Saved queries
* Test

Specify a [custom schema](https://docs.getdbt.com/docs/build/custom-schemas.md#understanding-custom-schemas) for a group of models in your project YAML file (`dbt_project.yml`) or in a [SQL file config](https://docs.getdbt.com/reference/resource-configs/schema.md#models).

For example, if you have a group of marketing-related models and want to place them in a separate schema called `marketing`, you can configure it like this:

dbt\_project.yml

```yml
models:
  your_project:
    marketing: #  Grouping or folder for set of models
      +schema: marketing
```

This would result in the generated relations for these models being located in the `marketing` schema, so the full relation names would be `analytics.target_schema_marketing.model_name`. This is because the schema of the relation is `{{ target.schema }}_{{ schema }}`. The [definition](#definition) section explains this in more detail.

Configure a [custom schema](https://docs.getdbt.com/docs/build/custom-schemas.md#understanding-custom-schemas) in your `dbt_project.yml` file.

For example, if you have a seed that should be placed in a separate schema called `mappings`, you can configure it like this:

dbt\_project.yml

```yml
seeds:
  your_project:
    product_mappings:
      +schema: mappings
```

This would result in the generated relation being located in the `mappings` schema, so the full relation name would be `analytics.mappings.seed_name`.

Specify a [custom schema](https://docs.getdbt.com/docs/build/custom-schemas.md#understanding-custom-schemas) for a [saved query](https://docs.getdbt.com/docs/build/saved-queries.md#parameters) in your `dbt_project.yml` or property file.

dbt\_project.yml

```yml
saved-queries:
  +schema: metrics
```

This would result in the saved query being stored in the `metrics` schema.

Customize a [custom schema](https://docs.getdbt.com/docs/build/custom-schemas.md#understanding-custom-schemas) for storing test results in your `dbt_project.yml` file.

For example, to save test results in a specific schema, you can configure it like this:

dbt\_project.yml

```yml
data_tests:
  +store_failures: true
  +schema: test_results
```

This would result in the test results being stored in the `test_results` schema.

Refer to [Usage](#usage) for more examples.

## Definition[​](#definition "Direct link to Definition")

Optionally specify a custom schema for a [model](https://docs.getdbt.com/docs/build/sql-models.md), [seed](https://docs.getdbt.com/docs/build/seeds.md), [snapshot](https://docs.getdbt.com/docs/build/snapshots.md), [saved query](https://docs.getdbt.com/docs/build/saved-queries.md), or [test](https://docs.getdbt.com/docs/build/data-tests.md).

For users on dbt v1.8 or earlier, use the [`target_schema` config](https://docs.getdbt.com/reference/resource-configs/target_schema.md) to specify a custom schema for a snapshot.

When dbt creates a relation (table/view) in a database, it creates it as: `{{ database }}.{{ schema }}.{{ identifier }}`, e.g. `analytics.finance.payments`

The standard behavior of dbt is:

* If a custom schema is *not* specified, the schema of the relation is the target schema (`{{ target.schema }}`).
* If a custom schema is specified, by default, the schema of the relation is `{{ target.schema }}_{{ schema }}`.

To learn more about changing the way that dbt generates a relation's `schema`, read [Using Custom Schemas](https://docs.getdbt.com/docs/build/custom-schemas.md)

## Usage[​](#usage "Direct link to Usage")

### Models[​](#models "Direct link to Models")

Configure groups of models from the `dbt_project.yml` file.

dbt\_project.yml

```yml
models:
  jaffle_shop: # the name of a project
    marketing:
      +schema: marketing
```

Configure individual models using a config block:

models/my\_model.sql

```sql
{{ config(
    schema='marketing'
) }}
```

### Seeds[​](#seeds "Direct link to Seeds")

dbt\_project.yml

```yml
seeds:
  +schema: mappings
```

### Data tests[​](#data-tests "Direct link to Data tests")

Customize the name of the schema in which tests [configured to store failures](https://docs.getdbt.com/reference/resource-configs/store_failures.md) will save their results. The resulting schema is `{{ profile.schema }}_{{ tests.schema }}`, with a default suffix of `dbt_test__audit`. To use the same profile schema, set `+schema: null`.

dbt\_project.yml

```yml
data_tests:
  +store_failures: true
  +schema: _sad_test_failures  # Will write tables to my_database.my_schema__sad_test_failures
```

Ensure you have the authorization to create or access schemas for your work. To ensure that the required schemas have the correct permissions, run a SQL statement in your respective data platform environment. For example, run the following command if using Redshift (exact authorization query may differ from one data platform to another):

```sql
create schema if not exists dev_username_dbt_test__audit authorization username;
```

*Replace `dev_username` with your specific development schema name and `username` with the appropriate user who should have the permissions.*

This command grants the appropriate permissions to create and access the `dbt_test__audit` schema, which is often used with the `store_failures` configuration.

## Warehouse specific information[​](#warehouse-specific-information "Direct link to Warehouse specific information")

* BigQuery: `dataset` and `schema` are interchangeable

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
