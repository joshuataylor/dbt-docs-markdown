# How did dbt choose which schema to build my models in?

By default, dbt builds models in your target schema. To change your target schema:

* If you're developing in **dbt**, these are set for each user when you first use a development environment.
* If you're developing with **dbt Core**, this is the `schema:` parameter in your `profiles.yml` file.

If you wish to split your models across multiple schemas, check out the docs on [using custom schemas](https://docs.getdbt.com/docs/build/custom-schemas.md).

Note: on BigQuery, `dataset` is used interchangeably with `schema`.
