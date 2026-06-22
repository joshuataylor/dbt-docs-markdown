# Add a seed file

1. Add a seed file:

seeds/country\_codes.csv

```text
country_code,country_name
US,United States
CA,Canada
GB,United Kingdom
...
```

2. Run `dbt seed`
3. Ref the model in a downstream model

models/something.sql

```sql
select * from {{ ref('country_codes') }}
```

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
