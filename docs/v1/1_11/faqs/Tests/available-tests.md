# What data tests are available for me to use in dbt?

Out of the box, dbt ships with the following data tests:

* `unique`
* `not_null`
* `accepted_values`
* `relationships` (for example, referential integrity)

You can also write your own [custom generic tests](https://docs.getdbt.com/docs/build/data-tests.md#generic-data-tests).

Some additional generic tests have been open-sourced in the [dbt-utils package](https://github.com/dbt-labs/dbt-utils#generic-tests). Check out the docs on [packages](https://docs.getdbt.com/docs/build/packages.md) to learn how to make these tests available in your project.
