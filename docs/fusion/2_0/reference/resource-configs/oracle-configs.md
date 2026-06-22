# Oracle configurations

## Use `parallel` hint[​](#use-parallel-hint "Direct link to use-parallel-hint")

Table materialization supports specifying the number of parallel executions as shown below

```sql
-- Create a dbt model using 4 parallel executions
{{config(materialized='table', parallel=4}}
SELECT c.cust_id, c.cust_first_name, c.cust_last_name
from {{ source('sh_database', 'customers') }} c
```

## Use `table_compression_clause`[​](#use-table_compression_clause "Direct link to use-table_compression_clause")

Table materialization supports different compression clauses as shown below

### Advanced Row Compression[​](#advanced-row-compression "Direct link to Advanced Row Compression")

With Advanced compression enabled, Oracle Database maintains compression during all types of data manipulation operations, including conventional DML such as INSERT and UPDATE. `ROW STORE COMPRESS ADVANCED` is recommended in OLTP systems.

```sql
-- Advanced Row compression
{{config(materialized='table', table_compression_clause='ROW STORE COMPRESS ADVANCED')}}
SELECT c.cust_id, c.cust_first_name, c.cust_last_name
from {{ source('sh_database', 'customers') }} c
```

### Hybrid Columnar Compression[​](#hybrid-columnar-compression "Direct link to Hybrid Columnar Compression")

#### Querying[​](#querying "Direct link to Querying")

`COLUMN STORE COMPRESS FOR QUERY` is useful in data warehouse environments. Valid values are `HIGH` or `LOW`, with `HIGH` providing a higher compression ratio. The default is `HIGH`

```sql
{{config(materialized='table', table_compression_clause='COLUMN STORE COMPRESS FOR QUERY LOW')}}
SELECT c.cust_id, c.cust_first_name, c.cust_last_name
from {{ source('sh_database', 'customers') }} c
```

or

```sql
{{config(materialized='table', table_compression_clause='COLUMN STORE COMPRESS FOR QUERY HIGH')}}
SELECT c.cust_id, c.cust_first_name, c.cust_last_name
from {{ source('sh_database', 'customers') }} c
```

#### Archival[​](#archival "Direct link to Archival")

`COLUMN STORE COMPRESS FOR ARCHIVE` supports a higher compression ratio than `COLUMN STORE COMPRESS FOR QUERY` and is useful for archival. Valid values are `HIGH` or `LOW` with `HIGH` providing the highest compression ratio. The default is `LOW`

```sql
{{config(materialized='table', table_compression_clause='COLUMN STORE COMPRESS FOR ARCHIVE LOW')}}
SELECT c.cust_id, c.cust_first_name, c.cust_last_name
from {{ source('sh_database', 'customers') }} c
```

or

```sql
{{config(materialized='table', table_compression_clause='COLUMN STORE COMPRESS FOR ARCHIVE HIGH')}}
SELECT c.cust_id, c.cust_first_name, c.cust_last_name
from {{ source('sh_database', 'customers') }} c
```

## Partitioning[​](#partitioning "Direct link to Partitioning")

Table and Incremental materialization configuration supports adding a partitioning clause:

```sql
{
    config(
        materialized='incremental',
        unique_key='group_id',
        parallel=4,
        partition_config={"clause": "PARTITION BY HASH(PROD_NAME) PARTITIONS 4"},
        table_compression_clause='COLUMN STORE COMPRESS FOR QUERY LOW')
}}
SELECT *
FROM {{ source('sh_database', 'sales') }}
```

## Session info in `v$session`[​](#session-info-in-vsession "Direct link to session-info-in-vsession")

Custom session information can be supplied under `session_info` in `profiles.yml`

```yaml
dbt_test:
   target: dev
   outputs:
      dev:
         type: oracle
         user: "{{ env_var('DBT_ORACLE_USER') }}"
         pass: "{{ env_var('DBT_ORACLE_PASSWORD') }}"
         database: "{{ env_var('DBT_ORACLE_DATABASE') }}"
         tns_name: "{{ env_var('DBT_ORACLE_TNS_NAME') }}"
         schema: "{{ env_var('DBT_ORACLE_SCHEMA') }}"
         threads: 4
         session_info:
            action: "dbt run"
            client_identifier: "dbt-unique-client-uuid"
            client_info: "dbt Python3.9 thin driver"
            module: "dbt-oracle-1.8.x"
```

This helps to track dbt sessions in the Database view [V$SESSION](https://docs.oracle.com/en/database/oracle/oracle-database/19/refrn/V-SESSION.html)

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
