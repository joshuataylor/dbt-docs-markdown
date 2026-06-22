# What privileges does my database user need to use dbt?

Your user will need to be able to:

* `select` from raw data in your warehouse (i.e. data to be transformed)
* `create` schemas, and therefore create tables/views within that schema¹
* read system views to generate documentation (i.e. views in `information_schema`)

On Postgres, Redshift, Databricks, and Snowflake, use a series of `grants` to ensure that your user has the correct privileges. Check out [example permissions](https://docs.getdbt.com/reference/database-permissions/about-database-permissions.md) for these warehouses.

On BigQuery, use the "BigQuery User" role to assign these privileges.

***

¹Alternatively, a separate user can create a schema for the dbt user, and then grant the user privileges to create within this schema. We generally recommend granting your dbt user the ability to create schemas, as it is less complicated to implement.

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
