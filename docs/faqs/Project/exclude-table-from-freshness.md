# How do I exclude a table from a freshness snapshot?

Some tables in a data source may be updated infrequently. If you've set a `freshness` property at the source level, this table is likely to fail checks.

To work around this, you can set the table's freshness to null (`freshness: null`) to "unset" the freshness for a particular table:

models/\<filename>.yml

```yaml
sources:
  - name: jaffle_shop
    database: raw
    schema: jaffle_shop
    config: 
      freshness:
        warn_after: {count: 12, period: hour}
        error_after: {count: 24, period: hour}
      loaded_at_field: _etl_loaded_at

    tables:
      - name: orders
      - name: product_skus
        config:
          freshness: null # do not check freshness for this table
```

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
