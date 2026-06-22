# Advanced configuration usage

## Alternative SQL file config syntax[​](#alternative-sql-file-config-syntax "Direct link to Alternative SQL file config syntax")

Some configurations may contain characters (e.g. dashes) that cannot be parsed as a Jinja argument. For example, the following would return an error:

```sql
{{ config(
    post-hook="grant select on {{ this }} to role reporter",
    materialized='table'
) }}

select ...
```

While dbt provides an alias for any core configurations (for example, you should use `pre_hook` instead of `pre-hook` in a config block), your dbt project may contain custom configurations without aliases.

If you want to specify these configurations inside of a model, use the alternative config block syntax:

models/events/base/base\_events.sql

```sql
{{
  config({
    "post-hook": "grant select on {{ this }} to role reporter",
    "materialized": "table"
  })
}}


select ...
```

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
