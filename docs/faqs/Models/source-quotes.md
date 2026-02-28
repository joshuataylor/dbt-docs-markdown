# I need to use quotes to select from my source, what should I do?

This is reasonably common on Snowflake in particular.

By default, dbt will not quote the database, schema, or identifier for the source tables that you've specified.

To force dbt to quote one of these values, use the [`quoting` property](https://docs.getdbt.com/reference/resource-properties/quoting.md):

models/\<filename>.yml

```yaml
sources:
  - name: jaffle_shop
    database: raw
    schema: jaffle_shop
    quoting:
      database: true
      schema: true
      identifier: true

    tables:
      - name: order_items
      - name: orders
        # This overrides the `jaffle_shop` quoting config
        quoting:
          identifier: false
```

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
