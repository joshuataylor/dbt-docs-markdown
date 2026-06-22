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
