# Cloudera Impala configurations

## Configuring tables[​](#configuring-tables "Direct link to Configuring tables")

When materializing a model as `table`, you may include several optional configs that are specific to the dbt-impala plugin, in addition to the standard [model configs](https://docs.getdbt.com/reference/model-configs.md).

| Option              | Description                                                           | Required? | Example                                                  |
| ------------------- | --------------------------------------------------------------------- | --------- | -------------------------------------------------------- |
| partition\_by       | partition by a column, typically a directory per partition is created | No        | partition\_by=\['name']                                  |
| sort\_by            | sort by a column                                                      | No        | sort\_by=\['age']                                        |
| row\_format         | format to be used when storing individual arows                       | No        | row\_format='delimited'                                  |
| stored\_as          | underlying storage format of the table                                | No        | stored\_as='PARQUET'                                     |
| location            | storage location, typically an hdfs path                              | No        | LOCATION='/user/etl/destination'                         |
| comment             | comment for the table                                                 | No        | comment='this is the cleanest model'                     |
| serde\_properties   | SerDes (\[de-]serialization) properties of table                      | No        | serde\_properties="('quoteChar'=''', 'escapeChar'='\\')" |
| tbl\_properties     | any metadata can be stored as key/value pair with the table           | No        | tbl\_properties="('dbt\_test'='1')"                      |
| is\_cached          | true or false - if this table is cached                               | No        | is\_cached=false (default)                               |
| cache\_pool         | cache pool name to use if is\_cached is set to true                   | No        |                                                          |
| replication\_factor | cache replication factor to use if is\_cached is set to true          | No        |                                                          |
| external            | is this an external table - true / false                              | No        | external=true                                            |
| table\_type         | indicates the type of the table - iceberg / kudu                      | No        | table\_type="iceberg"                                    |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

For Cloudera specific options for above parameters see documentation of CREATE TABLE (<https://docs.cloudera.com/documentation/enterprise/6/6.3/topics/impala_create_table.html>)

## Incremental models[​](#incremental-models "Direct link to Incremental models")

Supported modes for incremental model:

* **`append`** (default): Insert new records without updating or overwriting any existing data.
* **`insert_overwrite`**: For new records, insert data. When used along with partition clause, update data for changed record and insert data for new records.

Unsupported modes:

* **`unique_key`** This is not suppored option for incremental models in dbt-impala
* **`merge`**: Merge is not supported by the underlying warehouse, and hence not supported by dbt-impala

## Example: Using partition\_by config option[​](#example-using-partition_by-config-option "Direct link to Example: Using partition_by config option")

impala\_partition\_by.sql

```sql
{{
    config(
        materialized='table',
        unique_key='id',
        partition_by=['city'],
    )
}}

with source_data as (
     select 1 as id, "Name 1" as name, "City 1" as city,
     union all
     select 2 as id, "Name 2" as name, "City 2" as city,
     union all
     select 3 as id, "Name 3" as name, "City 2" as city,
     union all
     select 4 as id, "Name 4" as name, "City 1" as city,
)

select * from source_data
```

In the above example, a sample table is created with partition\_by and other config options. One thing to note when using partition\_by option is that the select query should always have the column name used in partition\_by option as the last one, as can be seen for the `city` column name used in the above query. If the partition\_by clause is not the same as the last column in select statement, Impala will flag an error when trying to create the model.

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
