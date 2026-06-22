# Upsolver configurations

## Supported Upsolver SQLake functionality[​](#supported-upsolver-sqlake-functionality "Direct link to Supported Upsolver SQLake functionality")

| COMMAND                | STATE         | MATERIALIZED     |
| ---------------------- | ------------- | ---------------- |
| SQL compute cluster    | not supported | -                |
| SQL connections        | supported     | connection       |
| SQL copy job           | supported     | incremental      |
| SQL merge job          | supported     | incremental      |
| SQL insert job         | supported     | incremental      |
| SQL materialized views | supported     | materializedview |
| Expectations           | supported     | incremental      |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

## Configs materialization[​](#configs-materialization "Direct link to Configs materialization")

| Config                 | Required | Materialization              | Description                                                                                    | Example                                                                                         |
| ---------------------- | -------- | ---------------------------- | ---------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------- |
| connection\_type       | Yes      | connection                   | Connection identifier: S3/GLUE\_CATALOG/KINESIS                                                | connection\_type='S3'                                                                           |
| connection\_options    | Yes      | connection                   | Dictionary of options supported by selected connection                                         | connection\_options={ 'aws\_role': 'aws\_role', 'external\_id': 'SAMPLES', 'read\_only': True } |
| incremental\_strategy  | No       | incremental                  | Define one of incremental strategies: merge/copy/insert. Default: copy                         | incremental\_strategy='merge'                                                                   |
| source                 | No       | incremental                  | Define source to copy from: S3/KAFKA/KINESIS                                                   | source = 'S3'                                                                                   |
| target\_type           | No       | incremental                  | Define target type REDSHIFT/ELASTICSEARCH/S3/SNOWFLAKE/POSTGRES. Default None for Data lake    | target\_type='Snowflake'                                                                        |
| target\_prefix         | False    | incremental                  | Define PREFIX for ELASTICSEARCH target type                                                    | target\_prefix = 'orders'                                                                       |
| target\_location       | False    | incremental                  | Define LOCATION for S3 target type                                                             | target\_location = 's3://your-bucket-name/path/to/folder/'                                      |
| schema                 | Yes/No   | incremental                  | Define target schema. Required if target\_type, no table created in a metastore connection     | schema = 'target\_schema'                                                                       |
| database               | Yes/No   | incremental                  | Define target connection. Required if target\_type, no table created in a metastore connection | database = 'target\_connection'                                                                 |
| alias                  | Yes/No   | incremental                  | Define target table. Required if target\_type, no table created in a metastore connection      | alias = 'target\_table'                                                                         |
| delete\_condition      | No       | incremental                  | Records that match the ON condition and a delete condition can be deleted                      | delete\_condition='nettotal > 1000'                                                             |
| partition\_by          | No       | incremental                  | List of dictionaries to define partition\_by for target metastore table                        | partition\_by=\[{'field':'$field\_name'}]                                                       |
| primary\_key           | No       | incremental                  | List of dictionaries to define partition\_by for target metastore table                        | primary\_key=\[{'field':'customer\_email', 'type':'string'}]                                    |
| map\_columns\_by\_name | No       | incremental                  | Maps columns from the SELECT statement to the table. Boolean. Default: False                   | map\_columns\_by\_name=True                                                                     |
| sync                   | No       | incremental/materializedview | Boolean option to define if job is synchronized or non-msynchronized. Default: False           | sync=True                                                                                       |
| options                | No       | incremental/materializedview | Dictionary of job options                                                                      | options={ 'START\_FROM': 'BEGINNING', 'ADD\_MISSING\_COLUMNS': True }                           |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

## SQL connection[​](#sql-connection "Direct link to SQL connection")

Connections are used to provide Upsolver with the proper credentials to bring your data into SQLake as well as to write out your transformed data to various services. More details on ["Upsolver SQL connections"](https://docs.upsolver.com/sqlake/sql-command-reference/sql-connections) As a dbt model connection is a model with materialized='connection'

```sql
{{ config(
        materialized='connection',
        connection_type={ 'S3' | 'GLUE_CATALOG' | 'KINESIS' | 'KAFKA'| 'SNOWFLAKE' },
        connection_options={}
        )
}}
```

Running this model will compile CREATE CONNECTION(or ALTER CONNECTION if exists) SQL and send it to Upsolver engine. Name of the connection will be name of the model.

## SQL copy job[​](#sql-copy-job "Direct link to SQL copy job")

A COPY FROM job allows you to copy your data from a given source into a table created in a metastore connection. This table then serves as your staging table and can be used with SQLake transformation jobs to write to various target locations. More details on ["Upsolver SQL copy-from"](https://docs.upsolver.com/sqlake/sql-command-reference/sql-jobs/create-job/copy-from)

As a dbt model copy job is model with materialized='incremental'

```sql
{{ config(  materialized='incremental',
            sync=True|False,
            source = 'S3'| 'KAFKA' | ... ,
            options={
              'option_name': 'option_value'
            },
            partition_by=[{}]
          )
}}
SELECT * FROM {{ ref(<model>) }}
```

Running this model will compile CREATE TABLE SQL for target type Data lake (or ALTER TABLE if exists) and CREATE COPY JOB(or ALTER COPY JOB if exists) SQL and send it to Upsolver engine. Name of the table will be name of the model. Name of the job will be name of the model plus '\_job'

## SQL insert job[​](#sql-insert-job "Direct link to SQL insert job")

An INSERT job defines a query that pulls in a set of data based on the given SELECT statement and inserts it into the designated target. This query is then run periodically based on the RUN\_INTERVAL defined within the job. More details on ["Upsolver SQL insert"](https://docs.upsolver.com/sqlake/sql-command-reference/sql-jobs/create-job/sql-transformation-jobs/insert).

As a dbt model insert job is model with materialized='incremental' and incremental\_strategy='insert'

```sql
{{ config(  materialized='incremental',
            sync=True|False,
            map_columns_by_name=True|False,
            incremental_strategy='insert',
            options={
              'option_name': 'option_value'
            },
            primary_key=[{}]
          )
}}
SELECT ...
FROM {{ ref(<model>) }}
WHERE ...
GROUP BY ...
HAVING COUNT(DISTINCT orderid::string) ...
```

Running this model will compile CREATE TABLE SQL for target type Data lake(or ALTER TABLE if exists) and CREATE INSERT JOB(or ALTER INSERT JOB if exists) SQL and send it to Upsolver engine. Name of the table will be name of the model. Name of the job will be name of the model plus '\_job'

## SQL merge job[​](#sql-merge-job "Direct link to SQL merge job")

A MERGE job defines a query that pulls in a set of data based on the given SELECT statement and inserts into, replaces, or deletes the data from the designated target based on the job definition. This query is then run periodically based on the RUN\_INTERVAL defined within the job. More details on ["Upsolver SQL merge"](https://docs.upsolver.com/sqlake/sql-command-reference/sql-jobs/create-job/sql-transformation-jobs/merge).

As a dbt model merge job is model with materialized='incremental' and incremental\_strategy='merge'

```sql
{{ config(  materialized='incremental',
            sync=True|False,
            map_columns_by_name=True|False,
            incremental_strategy='merge',
            options={
              'option_name': 'option_value'
            },
            primary_key=[{}]
          )
}}
SELECT ...
FROM {{ ref(<model>) }}
WHERE ...
GROUP BY ...
HAVING COUNT ...
```

Running this model will compile CREATE TABLE SQL for target type Data lake(or ALTER TABLE if exists) and CREATE MERGE JOB(or ALTER MERGE JOB if exists) SQL and send it to Upsolver engine. Name of the table will be name of the model. Name of the job will be name of the model plus '\_job'

## SQL materialized views[​](#sql-materialized-views "Direct link to SQL materialized views")

When transforming your data, you may find that you need data from multiple source tables in order to achieve your desired result. In such a case, you can create a materialized view from one SQLake table in order to join it with your other table (which in this case is considered the main table). More details on ["Upsolver SQL materialized views"](https://docs.upsolver.com/sqlake/sql-command-reference/sql-jobs/create-job/sql-transformation-jobs/sql-materialized-views).

As a dbt model materialized views is model with materialized='materializedview'.

```sql
{{ config(  materialized='materializedview',
            sync=True|False,
            options={'option_name': 'option_value'}
        )
}}
SELECT ...
FROM {{ ref(<model>) }}
WHERE ...
GROUP BY ...
```

Running this model will compile CREATE MATERIALIZED VIEW SQL(or ALTER MATERIALIZED VIEW if exists) and send it to Upsolver engine. Name of the materializedview will be name of the model.

## Expectations/constraints[​](#expectationsconstraints "Direct link to Expectations/constraints")

Data quality conditions can be added to your job to drop a row or trigger a warning when a column violates a predefined condition.

```sql
WITH EXPECTATION <expectation_name> EXPECT <sql_predicate>
ON VIOLATION WARN
```

Expectations can be implemented with dbt constraints Supported constraints: check and not\_null

```yaml
models:
  - name: <model name>
    # required
    config:
      contract:
        enforced: true
    # model-level constraints
    constraints:
      - type: check
        columns: ['<column1>', '<column2>']
        expression: "column1 <= column2"
        name: <constraint_name>
      - type: not_null
        columns: ['column1', 'column2']
        name: <constraint_name>

    columns:
      - name: <column3>
        data_type: string

        # column-level constraints
        constraints:
          - type: not_null
          - type: check
            expression: "REGEXP_LIKE(<column3>, '^[0-9]{4}[a-z]{5}$')"
            name: <constraint_name>
```

## Projects examples[​](#projects-examples "Direct link to Projects examples")

> projects examples link: [github.com/dbt-upsolver/examples/](https://github.com/Upsolver/dbt-upsolver/tree/main/examples)

## Connection options[​](#connection-options "Direct link to Connection options")

| Option                             | Storage       | Editable | Optional | Config Syntax                                                       |
| ---------------------------------- | ------------- | -------- | -------- | ------------------------------------------------------------------- |
| aws\_role                          | s3            | True     | True     | 'aws\_role': `'<aws_role>'`                                         |
| external\_id                       | s3            | True     | True     | 'external\_id': `'<external_id>'`                                   |
| aws\_access\_key\_id               | s3            | True     | True     | 'aws\_access\_key\_id': `'<aws_access_key_id>'`                     |
| aws\_secret\_access\_key           | s3            | True     | True     | 'aws\_secret\_access\_key\_id': `'<aws_secret_access_key_id>'`      |
| path\_display\_filter              | s3            | True     | True     | 'path\_display\_filter': `'<path_display_filter>'`                  |
| path\_display\_filters             | s3            | True     | True     | 'path\_display\_filters': (`'<filter>'`, ...)                       |
| read\_only                         | s3            | True     | True     | 'read\_only': True/False                                            |
| encryption\_kms\_key               | s3            | True     | True     | 'encryption\_kms\_key': `'<encryption_kms_key>'`                    |
| encryption\_customer\_managed\_key | s3            | True     | True     | 'encryption\_customer\_kms\_key': `'<encryption_customer_kms_key>'` |
| comment                            | s3            | True     | True     | 'comment': `'<comment>'`                                            |
| host                               | kafka         | False    | False    | 'host': `'<host>'`                                                  |
| hosts                              | kafka         | False    | False    | 'hosts': (`'<host>'`, ...)                                          |
| consumer\_properties               | kafka         | True     | True     | 'consumer\_properties': `'<consumer_properties>'`                   |
| version                            | kafka         | False    | True     | 'version': `'<value>'`                                              |
| require\_static\_ip                | kafka         | True     | True     | 'require\_static\_ip': True/False                                   |
| ssl                                | kafka         | True     | True     | 'ssl': True/False                                                   |
| topic\_display\_filter             | kafka         | True     | True     | 'topic\_display\_filter': `'<topic_display_filter>'`                |
| topic\_display\_filters            | kafka         | True     | True     | 'topic\_display\_filter': (`'<filter>'`, ...)                       |
| comment                            | kafka         | True     | True     | 'comment': `'<comment>'`                                            |
| aws\_role                          | glue\_catalog | True     | True     | 'aws\_role': `'<aws_role>'`                                         |
| external\_id                       | glue\_catalog | True     | True     | 'external\_id': `'<external_id>'`                                   |
| aws\_access\_key\_id               | glue\_catalog | True     | True     | 'aws\_access\_key\_id': `'<aws_access_key_id>'`                     |
| aws\_secret\_access\_key           | glue\_catalog | True     | True     | 'aws\_secret\_access\_key': `'<aws_secret_access_key>'`             |
| default\_storage\_connection       | glue\_catalog | False    | False    | 'default\_storage\_connection': `'<default_storage_connection>'`    |
| default\_storage\_location         | glue\_catalog | False    | False    | 'default\_storage\_location': `'<default_storage_location>'`        |
| region                             | glue\_catalog | False    | True     | 'region': `'<region>'`                                              |
| database\_display\_filter          | glue\_catalog | True     | True     | 'database\_display\_filter': `'<database_display_filter>'`          |
| database\_display\_filters         | glue\_catalog | True     | True     | 'database\_display\_filters': (`'<filter>'`, ...)                   |
| comment                            | glue\_catalog | True     | True     | 'comment': `'<comment>'`                                            |
| aws\_role                          | kinesis       | True     | True     | 'aws\_role': `'<aws_role>'`                                         |
| external\_id                       | kinesis       | True     | True     | 'external\_id': `'<external_id>'`                                   |
| aws\_access\_key\_id               | kinesis       | True     | True     | 'aws\_access\_key\_id': `'<aws_access_key_id>'`                     |
| aws\_secret\_access\_key           | kinesis       | True     | True     | 'aws\_secret\_access\_key': `'<aws_secret_access_key>'`             |
| region                             | kinesis       | False    | False    | 'region': `'<region>'`                                              |
| read\_only                         | kinesis       | False    | True     | 'read\_only': True/False                                            |
| max\_writers                       | kinesis       | True     | True     | 'max\_writers': `<integer>`                                         |
| stream\_display\_filter            | kinesis       | True     | True     | 'stream\_display\_filter': `'<stream_display_filter>'`              |
| stream\_display\_filters           | kinesis       | True     | True     | 'stream\_display\_filters': (`'<filter>'`, ...)                     |
| comment                            | kinesis       | True     | True     | 'comment': `'<comment>'`                                            |
| connection\_string                 | snowflake     | True     | False    | 'connection\_string': `'<connection_string>'`                       |
| user\_name                         | snowflake     | True     | False    | 'user\_name': `'<user_name>'`                                       |
| password                           | snowflake     | True     | False    | 'password': `'<password>'`                                          |
| max\_concurrent\_connections       | snowflake     | True     | True     | 'max\_concurrent\_connections': `<integer>`                         |
| comment                            | snowflake     | True     | True     | 'comment': `'<comment>'`                                            |
| connection\_string                 | redshift      | True     | False    | 'connection\_string': `'<connection_string>'`                       |
| user\_name                         | redshift      | True     | False    | 'user\_name': `'<user_name>'`                                       |
| password                           | redshift      | True     | False    | 'password': `'<password>'`                                          |
| max\_concurrent\_connections       | redshift      | True     | True     | 'max\_concurrent\_connections': `<integer>`                         |
| comment                            | redshift      | True     | True     | 'comment': `'<comment>'`                                            |
| connection\_string                 | mysql         | True     | False    | 'connection\_string': `'<connection_string>'`                       |
| user\_name                         | mysql         | True     | False    | 'user\_name': `'<user_name>'`                                       |
| password                           | mysql         | True     | False    | 'password': `'<password>'`                                          |
| comment                            | mysql         | True     | True     | 'comment': `'<comment>'`                                            |
| connection\_string                 | postgres      | True     | False    | 'connection\_string': `'<connection_string>'`                       |
| user\_name                         | postgres      | True     | False    | 'user\_name': `'<user_name>'`                                       |
| password                           | postgres      | True     | False    | 'password': `'<password>'`                                          |
| comment                            | postgres      | True     | True     | 'comment': `'<comment>'`                                            |
| connection\_string                 | elasticsearch | True     | False    | 'connection\_string': `'<connection_string>'`                       |
| user\_name                         | elasticsearch | True     | False    | 'user\_name': `'<user_name>'`                                       |
| password                           | elasticsearch | True     | False    | 'password': `'<password>'`                                          |
| comment                            | elasticsearch | True     | True     | 'comment': `'<comment>'`                                            |
| connection\_string                 | mongodb       | True     | False    | 'connection\_string': `'<connection_string>'`                       |
| user\_name                         | mongodb       | True     | False    | 'user\_name': `'<user_name>'`                                       |
| password                           | mongodb       | True     | False    | 'password': `'<password>'`                                          |
| timeout                            | mongodb       | True     | True     | 'timeout': "INTERVAL 'N' SECONDS"                                   |
| comment                            | mongodb       | True     | True     | 'comment': `'<comment>'`                                            |
| connection\_string                 | mssql         | True     | False    | 'connection\_string': `'<connection_string>'`                       |
| user\_name                         | mssql         | True     | False    | 'user\_name': `'<user_name>'`                                       |
| password                           | mssql         | True     | False    | 'password': `'<password>'`                                          |
| comment                            | mssql         | True     | True     | 'comment': `'<comment>'`                                            |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

## Target options[​](#target-options "Direct link to Target options")

| Option                      | Storage            | Editable | Optional | Config Syntax                                                                   |
| --------------------------- | ------------------ | -------- | -------- | ------------------------------------------------------------------------------- |
| globally\_unique\_keys      | datalake           | False    | True     | 'globally\_unique\_keys': True/False                                            |
| storage\_connection         | datalake           | False    | True     | 'storage\_connection': `'<storage_connection>'`                                 |
| storage\_location           | datalake           | False    | True     | 'storage\_location': `'<storage_location>'`                                     |
| compute\_cluster            | datalake           | True     | True     | 'compute\_cluster': `'<compute_cluster>'`                                       |
| compression                 | datalake           | True     | True     | 'compression': 'SNAPPY/GZIP'                                                    |
| compaction\_processes       | datalake           | True     | True     | 'compaction\_processes': `<integer>`                                            |
| disable\_compaction         | datalake           | True     | True     | 'disable\_compaction': True/False                                               |
| retention\_date\_partition  | datalake           | False    | True     | 'retention\_date\_partition': `'<column>'`                                      |
| table\_data\_retention      | datalake           | True     | True     | 'table\_data\_retention': `'<N DAYS>'`                                          |
| column\_data\_retention     | datalake           | True     | True     | 'column\_data\_retention': ({'COLUMN' : `'<column>'`,'DURATION': `'<N DAYS>'`}) |
| comment                     | datalake           | True     | True     | 'comment': `'<comment>'`                                                        |
| storage\_connection         | materialized\_view | False    | True     | 'storage\_connection': `'<storage_connection>'`                                 |
| storage\_location           | materialized\_view | False    | True     | 'storage\_location': `'<storage_location>'`                                     |
| max\_time\_travel\_duration | materialized\_view | True     | True     | 'max\_time\_travel\_duration': `'<N DAYS>'`                                     |
| compute\_cluster            | materialized\_view | True     | True     | 'compute\_cluster': `'<compute_cluster>'`                                       |
| column\_transformations     | snowflake          | False    | True     | 'column\_transformations': {`'<column>'` : `'<expression>'` , ...}              |
| deduplicate\_with           | snowflake          | False    | True     | 'deduplicate\_with': {'COLUMNS' : \['col1', 'col2'],'WINDOW': 'N HOURS'}        |
| exclude\_columns            | snowflake          | False    | True     | 'exclude\_columns': (`'<exclude_column>'`, ...)                                 |
| create\_table\_if\_missing  | snowflake          | False    | True     | 'create\_table\_if\_missing': True/False}                                       |
| run\_interval               | snowflake          | False    | True     | 'run\_interval': `'<N MINUTES/HOURS/DAYS>'`                                     |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

## Transformation options[​](#transformation-options "Direct link to Transformation options")

| Option                             | Storage       | Editable | Optional | Config Syntax                                                                                     |
| ---------------------------------- | ------------- | -------- | -------- | ------------------------------------------------------------------------------------------------- |
| run\_interval                      | s3            | False    | True     | 'run\_interval': `'<N MINUTES/HOURS/DAYS>'`                                                       |
| start\_from                        | s3            | False    | True     | 'start\_from': `'<timestamp>/NOW/BEGINNING'`                                                      |
| end\_at                            | s3            | True     | True     | 'end\_at': `'<timestamp>/NOW'`                                                                    |
| compute\_cluster                   | s3            | True     | True     | 'compute\_cluster': `'<compute_cluster>'`                                                         |
| comment                            | s3            | True     | True     | 'comment': `'<comment>'`                                                                          |
| skip\_validations                  | s3            | False    | True     | 'skip\_validations': ('ALLOW\_CARTESIAN\_PRODUCT', ...)                                           |
| skip\_all\_validations             | s3            | False    | True     | 'skip\_all\_validations': True/False                                                              |
| aggregation\_parallelism           | s3            | True     | True     | 'aggregation\_parallelism': `<integer>`                                                           |
| run\_parallelism                   | s3            | True     | True     | 'run\_parallelism': `<integer>`                                                                   |
| file\_format                       | s3            | False    | False    | 'file\_format': '(type = `<file_format>`)'                                                        |
| compression                        | s3            | False    | True     | 'compression': 'SNAPPY/GZIP ...'                                                                  |
| date\_pattern                      | s3            | False    | True     | 'date\_pattern': `'<date_pattern>'`                                                               |
| output\_offset                     | s3            | False    | True     | 'output\_offset': `'<N MINUTES/HOURS/DAYS>'`                                                      |
| run\_interval                      | elasticsearch | False    | True     | 'run\_interval': `'<N MINUTES/HOURS/DAYS>'`                                                       |
| routing\_field\_name               | elasticsearch | True     | True     | 'routing\_field\_name': `'<routing_field_name>'`                                                  |
| start\_from                        | elasticsearch | False    | True     | 'start\_from': `'<timestamp>/NOW/BEGINNING'`                                                      |
| end\_at                            | elasticsearch | True     | True     | 'end\_at': `'<timestamp>/NOW'`                                                                    |
| compute\_cluster                   | elasticsearch | True     | True     | 'compute\_cluster': `'<compute_cluster>'`                                                         |
| skip\_validations                  | elasticsearch | False    | True     | 'skip\_validations': ('ALLOW\_CARTESIAN\_PRODUCT', ...)                                           |
| skip\_all\_validations             | elasticsearch | False    | True     | 'skip\_all\_validations': True/False                                                              |
| aggregation\_parallelism           | elasticsearch | True     | True     | 'aggregation\_parallelism': `<integer>`                                                           |
| run\_parallelism                   | elasticsearch | True     | True     | 'run\_parallelism': `<integer>`                                                                   |
| bulk\_max\_size\_bytes             | elasticsearch | True     | True     | 'bulk\_max\_size\_bytes': `<integer>`                                                             |
| index\_partition\_size             | elasticsearch | True     | True     | 'index\_partition\_size': 'HOURLY/DAILY ...'                                                      |
| comment                            | elasticsearch | True     | True     | 'comment': `'<comment>'`                                                                          |
| custom\_insert\_expressions        | snowflake     | True     | True     | 'custom\_insert\_expressions': {'INSERT\_TIME' : 'CURRENT\_TIMESTAMP()','MY\_VALUE': `'<value>'`} |
| custom\_update\_expressions        | snowflake     | True     | True     | 'custom\_update\_expressions': {'UPDATE\_TIME' : 'CURRENT\_TIMESTAMP()','MY\_VALUE': `'<value>'`} |
| keep\_existing\_values\_when\_null | snowflake     | True     | True     | 'keep\_existing\_values\_when\_null': True/False                                                  |
| add\_missing\_columns              | snowflake     | False    | True     | 'add\_missing\_columns': True/False                                                               |
| run\_interval                      | snowflake     | False    | True     | 'run\_interval': `'<N MINUTES/HOURS/DAYS>'`                                                       |
| commit\_interval                   | snowflake     | True     | True     | 'commit\_interval': `'<N MINUTE[S]/HOUR[S]/DAY[S]>'`                                              |
| start\_from                        | snowflake     | False    | True     | 'start\_from': `'<timestamp>/NOW/BEGINNING'`                                                      |
| end\_at                            | snowflake     | True     | True     | 'end\_at': `'<timestamp>/NOW'`                                                                    |
| compute\_cluster                   | snowflake     | True     | True     | 'compute\_cluster': `'<compute_cluster>'`                                                         |
| skip\_validations                  | snowflake     | False    | True     | 'skip\_validations': ('ALLOW\_CARTESIAN\_PRODUCT', ...)                                           |
| skip\_all\_validations             | snowflake     | False    | True     | 'skip\_all\_validations': True/False                                                              |
| aggregation\_parallelism           | snowflake     | True     | True     | 'aggregation\_parallelism': `<integer>`                                                           |
| run\_parallelism                   | snowflake     | True     | True     | 'run\_parallelism': `<integer>`                                                                   |
| comment                            | snowflake     | True     | True     | 'comment': `'<comment>'`                                                                          |
| add\_missing\_columns              | datalake      | False    | True     | 'add\_missing\_columns': True/False                                                               |
| run\_interval                      | datalake      | False    | True     | 'run\_interval': `'<N MINUTES/HOURS/DAYS>'`                                                       |
| start\_from                        | datalake      | False    | True     | 'start\_from': `'<timestamp>/NOW/BEGINNING'`                                                      |
| end\_at                            | datalake      | True     | True     | 'end\_at': `'<timestamp>/NOW'`                                                                    |
| compute\_cluster                   | datalake      | True     | True     | 'compute\_cluster': `'<compute_cluster>'`                                                         |
| skip\_validations                  | datalake      | False    | True     | 'skip\_validations': ('ALLOW\_CARTESIAN\_PRODUCT', ...)                                           |
| skip\_all\_validations             | datalake      | False    | True     | 'skip\_all\_validations': True/False                                                              |
| aggregation\_parallelism           | datalake      | True     | True     | 'aggregation\_parallelism': `<integer>`                                                           |
| run\_parallelism                   | datalake      | True     | True     | 'run\_parallelism': `<integer>`                                                                   |
| comment                            | datalake      | True     | True     | 'comment': `'<comment>'`                                                                          |
| run\_interval                      | redshift      | False    | True     | 'run\_interval': `'<N MINUTES/HOURS/DAYS>'`                                                       |
| start\_from                        | redshift      | False    | True     | 'start\_from': `'<timestamp>/NOW/BEGINNING'`                                                      |
| end\_at                            | redshift      | True     | True     | 'end\_at': `'<timestamp>/NOW'`                                                                    |
| compute\_cluster                   | redshift      | True     | True     | 'compute\_cluster': `'<compute_cluster>'`                                                         |
| skip\_validations                  | redshift      | False    | True     | 'skip\_validations': ('ALLOW\_CARTESIAN\_PRODUCT', ...)                                           |
| skip\_all\_validations             | redshift      | False    | True     | 'skip\_all\_validations': True/False                                                              |
| aggregation\_parallelism           | redshift      | True     | True     | 'aggregation\_parallelism': `<integer>`                                                           |
| run\_parallelism                   | redshift      | True     | True     | 'run\_parallelism': `<integer>`                                                                   |
| skip\_failed\_files                | redshift      | False    | True     | 'skip\_failed\_files': True/False                                                                 |
| fail\_on\_write\_error             | redshift      | False    | True     | 'fail\_on\_write\_error': True/False                                                              |
| comment                            | redshift      | True     | True     | 'comment': `'<comment>'`                                                                          |
| run\_interval                      | postgres      | False    | True     | 'run\_interval': `'<N MINUTES/HOURS/DAYS>'`                                                       |
| start\_from                        | postgres      | False    | True     | 'start\_from': `'<timestamp>/NOW/BEGINNING'`                                                      |
| end\_at                            | postgres      | True     | True     | 'end\_at': `'<timestamp>/NOW'`                                                                    |
| compute\_cluster                   | postgres      | True     | True     | 'compute\_cluster': `'<compute_cluster>'`                                                         |
| skip\_validations                  | postgres      | False    | True     | 'skip\_validations': ('ALLOW\_CARTESIAN\_PRODUCT', ...)                                           |
| skip\_all\_validations             | postgres      | False    | True     | 'skip\_all\_validations': True/False                                                              |
| aggregation\_parallelism           | postgres      | True     | True     | 'aggregation\_parallelism': `<integer>`                                                           |
| run\_parallelism                   | postgres      | True     | True     | 'run\_parallelism': `<integer>`                                                                   |
| comment                            | postgres      | True     | True     | 'comment': `'<comment>'`                                                                          |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

## Copy options[​](#copy-options "Direct link to Copy options")

| Option                     | Storage  | Category        | Editable | Optional | Config Syntax                                                            |
| -------------------------- | -------- | --------------- | -------- | -------- | ------------------------------------------------------------------------ |
| topic                      | kafka    | source\_options | False    | False    | 'topic': `'<topic>'`                                                     |
| exclude\_columns           | kafka    | job\_options    | False    | True     | 'exclude\_columns': (`'<exclude_column>'`, ...)                          |
| deduplicate\_with          | kafka    | job\_options    | False    | True     | 'deduplicate\_with': {'COLUMNS' : \['col1', 'col2'],'WINDOW': 'N HOURS'} |
| consumer\_properties       | kafka    | job\_options    | True     | True     | 'consumer\_properties': `'<consumer_properties>'`                        |
| reader\_shards             | kafka    | job\_options    | True     | True     | 'reader\_shards': `<integer>`                                            |
| store\_raw\_data           | kafka    | job\_options    | False    | True     | 'store\_raw\_data': True/False                                           |
| start\_from                | kafka    | job\_options    | False    | True     | 'start\_from': 'BEGINNING/NOW'                                           |
| end\_at                    | kafka    | job\_options    | True     | True     | 'end\_at': `'<timestamp>/NOW'`                                           |
| compute\_cluster           | kafka    | job\_options    | True     | True     | 'compute\_cluster': `'<compute_cluster>'`                                |
| run\_parallelism           | kafka    | job\_options    | True     | True     | 'run\_parallelism': `<integer>`                                          |
| content\_type              | kafka    | job\_options    | True     | True     | 'content\_type': 'AUTO/CSV/...'                                          |
| compression                | kafka    | job\_options    | False    | True     | 'compression': 'AUTO/GZIP/...'                                           |
| column\_transformations    | kafka    | job\_options    | False    | True     | 'column\_transformations': {`'<column>'` : `'<expression>'` , ...}       |
| commit\_interval           | kafka    | job\_options    | True     | True     | 'commit\_interval': `'<N MINUTE[S]/HOUR[S]/DAY[S]>'`                     |
| skip\_validations          | kafka    | job\_options    | False    | True     | 'skip\_validations': ('MISSING\_TOPIC')                                  |
| skip\_all\_validations     | kafka    | job\_options    | False    | True     | 'skip\_all\_validations': True/False                                     |
| comment                    | kafka    | job\_options    | True     | True     | 'comment': `'<comment>'`                                                 |
| table\_include\_list       | mysql    | source\_options | True     | True     | 'table\_include\_list': (`'<regexFilter>'`, ...)                         |
| column\_exclude\_list      | mysql    | source\_options | True     | True     | 'column\_exclude\_list': (`'<regexFilter>'`, ...)                        |
| exclude\_columns           | mysql    | job\_options    | False    | True     | 'exclude\_columns': (`'<exclude_column>'`, ...)                          |
| column\_transformations    | mysql    | job\_options    | False    | True     | 'column\_transformations': {`'<column>'` : `'<expression>'` , ...}       |
| skip\_snapshots            | mysql    | job\_options    | True     | True     | 'skip\_snapshots': True/False                                            |
| end\_at                    | mysql    | job\_options    | True     | True     | 'end\_at': `'<timestamp>/NOW'`                                           |
| compute\_cluster           | mysql    | job\_options    | True     | True     | 'compute\_cluster': `'<compute_cluster>'`                                |
| snapshot\_parallelism      | mysql    | job\_options    | True     | True     | 'snapshot\_parallelism': `<integer>`                                     |
| ddl\_filters               | mysql    | job\_options    | False    | True     | 'ddl\_filters': (`'<filter>'`, ...)                                      |
| comment                    | mysql    | job\_options    | True     | True     | 'comment': `'<comment>'`                                                 |
| table\_include\_list       | postgres | source\_options | False    | False    | 'table\_include\_list': (`'<regexFilter>'`, ...)                         |
| column\_exclude\_list      | postgres | source\_options | False    | True     | 'column\_exclude\_list': (`'<regexFilter>'`, ...)                        |
| heartbeat\_table           | postgres | job\_options    | False    | True     | 'heartbeat\_table': `'<heartbeat_table>'`                                |
| skip\_snapshots            | postgres | job\_options    | False    | True     | 'skip\_snapshots': True/False                                            |
| publication\_name          | postgres | job\_options    | False    | False    | 'publication\_name': `'<publication_name>'`                              |
| end\_at                    | postgres | job\_options    | True     | True     | 'end\_at': `'<timestamp>/NOW'`                                           |
| compute\_cluster           | postgres | job\_options    | True     | True     | 'compute\_cluster': `'<compute_cluster>'`                                |
| comment                    | postgres | job\_options    | True     | True     | 'comment': `'<comment>'`                                                 |
| parse\_json\_columns       | postgres | job\_options    | False    | False    | 'parse\_json\_columns': True/False                                       |
| column\_transformations    | postgres | job\_options    | False    | True     | 'column\_transformations': {`'<column>'` : `'<expression>'` , ...}       |
| snapshot\_parallelism      | postgres | job\_options    | True     | True     | 'snapshot\_parallelism': `<integer>`                                     |
| exclude\_columns           | postgres | job\_options    | False    | True     | 'exclude\_columns': (`'<exclude_column>'`, ...)                          |
| location                   | s3       | source\_options | False    | False    | 'location': `'<location>'`                                               |
| date\_pattern              | s3       | job\_options    | False    | True     | 'date\_pattern': `'<date_pattern>'`                                      |
| file\_pattern              | s3       | job\_options    | False    | True     | 'file\_pattern': `'<file_pattern>'`                                      |
| initial\_load\_pattern     | s3       | job\_options    | False    | True     | 'initial\_load\_pattern': `'<initial_load_pattern>'`                     |
| initial\_load\_prefix      | s3       | job\_options    | False    | True     | 'initial\_load\_prefix': `'<initial_load_prefix>'`                       |
| delete\_files\_after\_load | s3       | job\_options    | False    | True     | 'delete\_files\_after\_load': True/False                                 |
| deduplicate\_with          | s3       | job\_options    | False    | True     | 'deduplicate\_with': {'COLUMNS' : \['col1', 'col2'],'WINDOW': 'N HOURS'} |
| end\_at                    | s3       | job\_options    | True     | True     | 'end\_at': `'<timestamp>/NOW'`                                           |
| start\_from                | s3       | job\_options    | False    | True     | 'start\_from': `'<timestamp>/NOW/BEGINNING'`                             |
| compute\_cluster           | s3       | job\_options    | True     | True     | 'compute\_cluster': `'<compute_cluster>'`                                |
| run\_parallelism           | s3       | job\_options    | True     | True     | 'run\_parallelism': `<integer>`                                          |
| content\_type              | s3       | job\_options    | True     | True     | 'content\_type': 'AUTO/CSV...'                                           |
| compression                | s3       | job\_options    | False    | True     | 'compression': 'AUTO/GZIP...'                                            |
| comment                    | s3       | job\_options    | True     | True     | 'comment': `'<comment>'`                                                 |
| column\_transformations    | s3       | job\_options    | False    | True     | 'column\_transformations': {`'<column>'` : `'<expression>'` , ...}       |
| commit\_interval           | s3       | job\_options    | True     | True     | 'commit\_interval': `'<N MINUTE[S]/HOUR[S]/DAY[S]>'`                     |
| skip\_validations          | s3       | job\_options    | False    | True     | 'skip\_validations': ('EMPTY\_PATH')                                     |
| skip\_all\_validations     | s3       | job\_options    | False    | True     | 'skip\_all\_validations': True/False                                     |
| exclude\_columns           | s3       | job\_options    | False    | True     | 'exclude\_columns': (`'<exclude_column>'`, ...)                          |
| stream                     | kinesis  | source\_options | False    | False    | 'stream': `'<stream>'`                                                   |
| reader\_shards             | kinesis  | job\_options    | True     | True     | 'reader\_shards': `<integer>`                                            |
| store\_raw\_data           | kinesis  | job\_options    | False    | True     | 'store\_raw\_data': True/False                                           |
| start\_from                | kinesis  | job\_options    | False    | True     | 'start\_from': `'<timestamp>/NOW/BEGINNING'`                             |
| end\_at                    | kinesis  | job\_options    | False    | True     | 'end\_at': `'<timestamp>/NOW'`                                           |
| compute\_cluster           | kinesis  | job\_options    | True     | True     | 'compute\_cluster': `'<compute_cluster>'`                                |
| run\_parallelism           | kinesis  | job\_options    | False    | True     | 'run\_parallelism': `<integer>`                                          |
| content\_type              | kinesis  | job\_options    | True     | True     | 'content\_type': 'AUTO/CSV...'                                           |
| compression                | kinesis  | job\_options    | False    | True     | 'compression': 'AUTO/GZIP...'                                            |
| comment                    | kinesis  | job\_options    | True     | True     | 'comment': `'<comment>'`                                                 |
| column\_transformations    | kinesis  | job\_options    | True     | True     | 'column\_transformations': {`'<column>'` : `'<expression>'` , ...}       |
| deduplicate\_with          | kinesis  | job\_options    | False    | True     | 'deduplicate\_with': {'COLUMNS' : \['col1', 'col2'],'WINDOW': 'N HOURS'} |
| commit\_interval           | kinesis  | job\_options    | True     | True     | 'commit\_interval': `'<N MINUTE[S]/HOUR[S]/DAY[S]>'`                     |
| skip\_validations          | kinesis  | job\_options    | False    | True     | 'skip\_validations': ('MISSING\_STREAM')                                 |
| skip\_all\_validations     | kinesis  | job\_options    | False    | True     | 'skip\_all\_validations': True/False                                     |
| exclude\_columns           | kinesis  | job\_options    | False    | True     | 'exclude\_columns': (`'<exclude_column>'`, ...)                          |
| table\_include\_list       | mssql    | source\_options | True     | True     | 'table\_include\_list': (`'<regexFilter>'`, ...)                         |
| column\_exclude\_list      | mssql    | source\_options | True     | True     | 'column\_exclude\_list': (`'<regexFilter>'`, ...)                        |
| exclude\_columns           | mssql    | job\_options    | False    | True     | 'exclude\_columns': (`'<exclude_column>'`, ...)                          |
| column\_transformations    | mssql    | job\_options    | False    | True     | 'column\_transformations': {`'<column>'` : `'<expression>'` , ...}       |
| skip\_snapshots            | mssql    | job\_options    | True     | True     | 'skip\_snapshots': True/False                                            |
| end\_at                    | mssql    | job\_options    | True     | True     | 'end\_at': `'<timestamp>/NOW'`                                           |
| compute\_cluster           | mssql    | job\_options    | True     | True     | 'compute\_cluster': `'<compute_cluster>'`                                |
| snapshot\_parallelism      | mssql    | job\_options    | True     | True     | 'snapshot\_parallelism': `<integer>`                                     |
| parse\_json\_columns       | mssql    | job\_options    | False    | False    | 'parse\_json\_columns': True/False                                       |
| comment                    | mssql    | job\_options    | True     | True     | 'comment': `'<comment>'`                                                 |
| collection\_include\_list  | mongodb  | source\_options | True     | True     | 'collection\_include\_list': (`'<regexFilter>'`, ...)                    |
| exclude\_columns           | mongodb  | job\_options    | False    | True     | 'exclude\_columns': (`'<exclude_column>'`, ...)                          |
| column\_transformations    | mongodb  | job\_options    | False    | True     | 'column\_transformations': {`'<column>'` : `'<expression>'` , ...}       |
| skip\_snapshots            | mongodb  | job\_options    | True     | True     | 'skip\_snapshots': True/False                                            |
| end\_at                    | mongodb  | job\_options    | True     | True     | 'end\_at': `'<timestamp>/NOW'`                                           |
| compute\_cluster           | mongodb  | job\_options    | True     | True     | 'compute\_cluster': `'<compute_cluster>'`                                |
| snapshot\_parallelism      | mongodb  | job\_options    | True     | True     | 'snapshot\_parallelism': `<integer>`                                     |
| comment                    | mongodb  | job\_options    | True     | True     | 'comment': `'<comment>'`                                                 |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
