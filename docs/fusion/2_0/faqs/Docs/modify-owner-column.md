# How do I populate the owner column in the generated docs?

You cannot change the `owner` column in your generated documentation.

dbt pulls the `owner` field in `dbt-docs` from database metadata ([catalog.json](https://docs.getdbt.com/reference/artifacts/catalog-json.md)), meaning the `owner` of that table in the database. With the exception of [exposures](https://docs.getdbt.com/docs/build/exposures.md), dbt does not pull this value from an `owner` field set within dbt.

Generally, dbt's database user owns the tables created in the database. The service responsible for ingesting or loading the data usually owns the source tables.

If you set `meta.owner`, that field appears under **meta** (pulled from dbt), but still not under the top-level `owner` field.

## Example[​](#example "Direct link to Example")

The following example shows a model with `meta.owner` so it appears under **meta** in the docs. Replace `DATA_TEAM_EMAIL` with your own values.

models/stg\_orders.yml

```yaml
models:
  - name: stg_orders
    description: "Staging table for order events."
    config:
      meta:
        owner: "DATA_TEAM_EMAIL"
    columns:
      - name: order_id
        description: "Primary key for orders."
      - name: order_date
        description: "Date when order was placed."
```

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
