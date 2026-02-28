# Set operators

### Unions[​](#unions "Direct link to Unions")

Providing multiple space-delineated arguments to the `--select` or `--exclude` flags selects the union of them all. If a resource is included in at least one selector, it will be included in the final set.

Run snowplow\_sessions, all ancestors of snowplow\_sessions, fct\_orders, and all ancestors of fct\_orders:

```bash
dbt run --select "+snowplow_sessions +fct_orders"
```

### Intersections[​](#intersections "Direct link to Intersections")

If you separate multiple arguments for `--select` and `--exclude` with commas and no whitespace in between, dbt will select only resources that satisfy *all* arguments.

Run all the common ancestors of snowplow\_sessions and fct\_orders:

```bash
dbt run --select "+snowplow_sessions,+fct_orders"
```

Run all the common descendents of stg\_invoices and stg\_accounts:

```bash
dbt run --select "stg_invoices+,stg_accounts+"
```

Run models that are in the marts/finance subdirectory *and* tagged nightly:

```bash
dbt run --select "marts.finance,tag:nightly"
```

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
