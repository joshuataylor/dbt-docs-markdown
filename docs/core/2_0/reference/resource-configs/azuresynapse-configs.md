# Microsoft Azure Synapse DWH configurations

All [configuration options for the Microsoft SQL Server adapter](https://docs.getdbt.com/reference/resource-configs/mssql-configs.md) also apply to this adapter.

Additionally, the configuration options below are available.

### Indices and distributions[​](#indices-and-distributions "Direct link to Indices and distributions")

The main index and the distribution type can be set for models that are materialized to tables.

* Model config
* Project config

models/example.sql

```sql
{{
    config(
        index='HEAP',
        dist='ROUND_ROBIN'
        )
}}

select *
from ...
```

dbt\_project.yml

```yaml
models:
  your_project_name:
    materialized: view
    staging:
      materialized: table
      index: HEAP
```

The following are the supported index types:

* `CLUSTERED COLUMNSTORE INDEX` (default)
* `HEAP`
* `CLUSTERED INDEX (COLUMN_NAME)`
* `CLUSTERED COLUMNSTORE INDEX ORDER(COLUMN_NAME)`

The following are the supported distribution types:

* `ROUND_ROBIN` (default)
* `HASH(COLUMN_NAME)`
* `REPLICATE`
