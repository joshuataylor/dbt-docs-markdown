# external

models/\<filename>.yml

```yml

sources:
  - name: <source_name>
    tables:
      - name: <table_name>
        external:
          location: <string>
          file_format: <string>
          row_format: <string>
          tbl_properties: <string>      
          partitions:
            - name: <column_name>
              data_type: <string>
              description: <string>
              meta: {dictionary}
            - ...
          <additional_property>: <additional_value>
```

Report incorrect code

## Definition

Use `external` on a source to describe a table that reads files in cloud storage, such as S3 or GCS. dbt stores these properties in the manifest, and the [`dbt-external-tables`](https://github.com/dbt-labs/dbt-external-tables) package uses them to create the table in your warehouse. There are optional built-in properties, with simple type validation, that roughly correspond to the Hive external table spec.

dbt doesn't validate most of these properties, so you can add your own too.

You may wish to define the `external` property in order to:

* Power macros that introspect [`graph.sources`](../dbt-jinja-functions/graph.md)
* Define metadata that you can later extract from the [manifest](../artifacts/manifest-json.md)

For an example of how this property can be used to power custom workflows, see the [`dbt-external-tables`](https://github.com/dbt-labs/dbt-external-tables) package.

## Partitions

A partition is a column whose value comes from the file's folder path, for example `dt` in `events/dt=2025-01-01/`. Declaring one lets your data platform skip folders you don't query.

Which properties you need depends on your data platform. For example:

| Data platform | Properties                                |
| ------------- | ----------------------------------------- |
| Snowflake     | `name`, `data_type`, `expression`         |
| BigQuery      | `name`, `data_type`                       |
| Redshift      | `name`, `data_type`, `vals`, `path_macro` |
| Spark         | `name`                                    |

For the full list, see the [`dbt-external-tables` sample sources](https://github.com/dbt-labs/dbt-external-tables/tree/main/sample_sources).

info

Keep `meta` directly on the partition. dbt doesn't move it under `config` for `external`, and `dbt-external-tables` doesn't read `config.meta`.

## Examples

The following examples show an external table over event files in cloud storage, partitioned by date (notice the different partition properties depending on your data platform).

### Snowflake

```yml
sources:
  - name: snowplow
    tables:
      - name: event_ext_tbl
        external:
          location: "@raw.snowplow.snowplow"  # an existing external stage
          file_format: "( type = json )"
          auto_refresh: true
          partitions:
            - name: collector_hour
              data_type: timestamp
              expression: to_timestamp(substr(metadata$filename, 8, 13), 'YYYY/MM/DD/HH24')
```

Report incorrect code

`metadata$filename` is the file path. The `expression` pulls the hour out of it.

### BigQuery

```yml
sources:
  - name: snowplow
    tables:
      - name: event
        external:
          location: 'gs://bucket/path/*'
          options:
            format: csv
            skip_leading_rows: 1
            hive_partition_uri_prefix: 'gs://bucket/path/'
          partitions:
            - name: collector_date
              data_type: date
```

Report incorrect code

BigQuery reads the partition from Hive-style folders, such as `gs://bucket/path/collector_date=2020-01-01/`. You only name the column and give its type.

### Redshift

```yml
sources:
  - name: snowplow
    tables:
      - name: event
        external:
          location: "s3://bucket/path"
          row_format: >
            serde 'org.openx.data.jsonserde.JsonSerDe'
          partitions:
            - name: appId
              data_type: varchar(255)
              vals:
                - dev
                - prod
              path_macro: dbt_external_tables.key_value
```

Report incorrect code

Redshift needs the values (`vals`) and a macro (`path_macro`) that turns each value into a folder path.
