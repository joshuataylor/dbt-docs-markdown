# Starrocks configurations

## Model Configuration[​](#model-configuration "Direct link to Model Configuration")

A dbt model can be configured using the following syntax:

* Project YAML file
* Properties YAML file
* SQL file config

dbt\_project.yml

```yaml
models:
  <resource-path>:
    materialized: table       // table or view or materialized_view
    keys: ['id', 'name', 'some_date']
    table_type: 'PRIMARY'     // PRIMARY or DUPLICATE or UNIQUE
    distributed_by: ['id']
    buckets: 3                // default 10
    partition_by: ['some_date']
    partition_by_init: ["PARTITION p1 VALUES [('1971-01-01 00:00:00'), ('1991-01-01 00:00:00')),PARTITION p1972 VALUES [('1991-01-01 00:00:00'), ('1999-01-01 00:00:00'))"]
    properties: [{"replication_num":"1", "in_memory": "true"}]
    refresh_method: 'async' // only for materialized view default manual
```

models/properties.yml

```yaml
models:
  - name: <model-name>
    config:
      materialized: table       // table or view or materialized_view
      keys: ['id', 'name', 'some_date']
      table_type: 'PRIMARY'     // PRIMARY or DUPLICATE or UNIQUE
      distributed_by: ['id']
      buckets: 3                // default 10
      partition_by: ['some_date']
      partition_by_init: ["PARTITION p1 VALUES [('1971-01-01 00:00:00'), ('1991-01-01 00:00:00')),PARTITION p1972 VALUES [('1991-01-01 00:00:00'), ('1999-01-01 00:00:00'))"]
      properties: [{"replication_num":"1", "in_memory": "true"}]
      refresh_method: 'async' // only for materialized view default manual
```

models/\<model\_name>.sql

```jinja
{{ config(
    materialized = 'table',
    keys=['id', 'name', 'some_date'],
    table_type='PRIMARY',
    distributed_by=['id'],
    buckets=3,
    partition_by=['some_date'],
    ....
) }}
```

### Configuration Description[​](#configuration-description "Direct link to Configuration Description")

| Option              | Description                                                                                                                                                                                  |
| ------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `materialized`      | How the model will be materialized into Starrocks. Supports view, table, incremental, ephemeral, and materialized\_view.                                                                     |
| `keys`              | Which columns serve as keys.                                                                                                                                                                 |
| `table_type`        | Table type, supported are PRIMARY or DUPLICATE or UNIQUE.                                                                                                                                    |
| `distributed_by`    | Specifies the column of data distribution. If not specified, it defaults to random.                                                                                                          |
| `buckets`           | The bucket number in one partition. If not specified, it will be automatically inferred.                                                                                                     |
| `partition_by`      | The partition column list.                                                                                                                                                                   |
| `partition_by_init` | The partition rule or some real partitions item.                                                                                                                                             |
| `properties`        | The table properties configuration of Starrocks. ([Starrocks table properties](https://docs.starrocks.io/en-us/latest/sql-reference/sql-statements/data-definition/CREATE_TABLE#properties)) |
| `refresh_method`    | How to refresh materialized views.                                                                                                                                                           |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

## Read From Catalog[​](#read-from-catalog "Direct link to Read From Catalog")

First you need to add this catalog to starrocks. The following is an example of hive.

```sql
CREATE EXTERNAL CATALOG `hive_catalog`
PROPERTIES (
    "hive.metastore.uris"  =  "thrift://127.0.0.1:8087",
    "type"="hive"
);
```

How to add other types of catalogs can be found in the documentation. [Catalog Overview](https://docs.starrocks.io/en-us/latest/data_source/catalog/catalog_overview) Then write the sources.yaml file.

```yaml
sources:
  - name: external_example
    schema: hive_catalog.hive_db
    tables:
      - name: hive_table_name
```

Finally, you might use below marco quote

```jinja
{{ source('external_example', 'hive_table_name') }}
```

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
