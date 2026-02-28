# database

* Model
* Seeds
* Snapshots
* Tests

Specify a custom database for a model in your `dbt_project.yml` file.

For example, if you have a model that you want to load into a database other than the target database, you can configure it like this:

dbt\_project.yml

```yml
models:
  your_project:
    sales_metrics:
      +database: reporting
```

This would result in the generated relation being located in the `reporting` database, so the full relation name would be `reporting.finance.sales_metrics` instead of the default target database.

Configure a database in your `dbt_project.yml` file.

For example, to load a seed into a database called `staging` instead of the target database, you can configure it like this:

dbt\_project.yml

```yml
seeds:
  your_project:
    product_categories:
      +database: staging
```

This would result in the generated relation being located in the `staging` database, so the full relation name would be `staging.finance.product_categories`.

Customize the database for storing test results in your `dbt_project.yml` file.

For example, to save test results in a specific database, you can configure it like this:

dbt\_project.yml

```yml
data_tests:
  +store_failures: true
  +database: test_results
```

This would result in the test results being stored in the `test_results` database.

## Definition[​](#definition "Direct link to Definition")

Optionally specify a custom database for a [model](https://docs.getdbt.com/docs/build/sql-models.md), [seed](https://docs.getdbt.com/docs/build/seeds.md), [snapshot](https://docs.getdbt.com/docs/build/snapshots.md), or [data test](https://docs.getdbt.com/docs/build/data-tests.md).

When dbt creates a relation (table/view) in a database, it creates it as: `{{ database }}.{{ schema }}.{{ identifier }}`, e.g. `analytics.finance.payments`

The standard behavior of dbt is:

* If a custom database is *not* specified, the database of the relation is the target database (`{{ target.database }}`).
* If a custom database is specified, the database of the relation is the `{{ database }}` value.

To learn more about changing the way that dbt generates a relation's `database`, read [Using Custom Databases](https://docs.getdbt.com/docs/build/custom-databases.md)

## Warehouse specific information[​](#warehouse-specific-information "Direct link to Warehouse specific information")

* BigQuery: `project` and `database` are interchangeable
* Databricks: `catalog` and `database` are interchangable

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
