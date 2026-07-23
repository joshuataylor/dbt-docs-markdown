# Connect DuckDB to dbt Core

[Fusion compatible](https://docs.getdbt.com/docs/local/connect-data-platform/duckdb-setup.md?version=2 "Fusion compatible") connection also available.

Community plugin

Some functionality may be limited. If you're interested in contributing, check out the source code for each repository listed below.

* **Maintained by**:
  <!-- -->
  Community
* **Authors**:
  <!-- -->
  Josh Wills (https\://github.com/jwills)
* **GitHub repo**: [duckdb/dbt-duckdb](https://github.com/duckdb/dbt-duckdb) [![](https://img.shields.io/github/stars/duckdb/dbt-duckdb?style=for-the-badge)](https://github.com/duckdb/dbt-duckdb)
* **PyPI package**: `dbt-duckdb` [![](https://badge.fury.io/py/dbt-duckdb.svg)](https://badge.fury.io/py/dbt-duckdb)
* **Slack channel**: [#db-duckdb](https://getdbt.slack.com/archives/C039D1J1LA2)
* **Supported dbt Core version**:
  <!-- -->
  v1.0.1
  <!-- -->
  and newer
* **dbt support**:
  <!-- -->
  Not Supported
* **Minimum data platform version**:
  <!-- -->
  DuckDB 0.3.2

## Installing <!-- -->dbt-duckdb

Use `pip` to install the adapter. Use the following command for installation:

`python -m pip install dbt-duckdb`

## Configuring <!-- -->dbt-duckdb<!-- -->

For <!-- -->Duck DB<!-- -->-specific configuration, please refer to [Duck DB<!-- --> configs.](https://docs.getdbt.com/reference/resource-configs/duckdb-configs.md)

## Connecting to DuckDB[​](#connecting-to-duckdb "Direct link to Connecting to DuckDB")

[DuckDB](https://duckdb.org) is an embedded database, similar to SQLite, but designed for OLAP-style analytics instead of OLTP. There are several ways to connect dbt to DuckDB depending on where you want your data to live. Configure your `profiles.yml` using the examples in the following sections:

* [In-memory](#in-memory)
* [Local file](#local-file)
* [MotherDuck](#motherduck)
* [Attaching additional databases](#attaching-additional-databases)

Refer to the following table for the fields to use in your `profiles.yml`. `type: duckdb` is *always* required. Use `path` for local files or MotherDuck connection strings, and use `:memory:` or omit `path` for an in-memory database.

| Profile field | Description                                                                                                    | Example                |
| ------------- | -------------------------------------------------------------------------------------------------------------- | ---------------------- |
| `type`        | The adapter type.                                                                                              | `duckdb`               |
| `path`        | Path to a DuckDB database file, a MotherDuck `md:` connection string, or `:memory:` for an in-memory database. | `./jaffle_shop.duckdb` |
| `schema`      | The schema name where dbt creates objects.                                                                     | `main`                 |
| `threads`     | Number of threads dbt uses when building models concurrently.                                                  | `4`                    |
| `extensions`  | List of [DuckDB extensions](https://duckdb.org/docs/extensions/overview) to load at startup.                   | `httpfs`, `parquet`    |
| `settings`    | Map of [DuckDB configuration options](https://duckdb.org/docs/sql/configuration) to set at startup.            | `s3_region: us-east-1` |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

If you're using Fusion, loading extensions requires you to install the DuckDB driver with [`dbc`](https://docs.columnar.tech/dbc/#__tabbed_2_4). Refer to [DuckDB driver and extensions](#driver-and-extensions) for details.

### In-memory[​](#in-memory "Direct link to In-memory")

The simplest configuration requires only `type: duckdb` in your profile. This runs an in-memory database — all data is lost after the run completes. This is useful for testing pipelines and for workflows that operate purely on external CSV, Parquet, or JSON files.

profiles.yml

```yaml
default:
  outputs:
    dev:
      type: duckdb
  target: dev
```

### Local file[​](#local-file "Direct link to Local file")

To persist data between runs, set `path` to a `.duckdb` file on your local filesystem. DuckDB creates the file automatically if it doesn't exist.

profiles.yml

```yaml
your_profile_name:
  target: dev
  outputs:
    dev:
      type: duckdb
      path: './my_project.duckdb'
      schema: main   # optional; defaults to main
      threads: 4     # optional
```

You can use a relative path (resolved relative to your `profiles.yml` file) or an absolute path. `dbt-duckdb` automatically sets the `database` property to the basename of the file with the suffix removed (for example, `/tmp/a/dbfile.duckdb` sets `database` to `dbfile`).

### MotherDuck[​](#motherduck "Direct link to MotherDuck")

In `dbt-duckdb 1.5.2` and later, you can connect to a DuckDB instance running on [MotherDuck](https://motherduck.com) by setting `path` to an `md:` connection string:

profiles.yml

```yaml
your_profile_name:
  target: dev
  outputs:
    dev:
      type: duckdb
      path: "md:my_db?motherduck_token={{ env_var('MOTHERDUCK_TOKEN') }}"
      threads: 4
```

MotherDuck databases generally work the same way as local DuckDB databases, with a few differences described in [MotherDuck's documentation](https://motherduck.com/docs/architecture-and-capabilities#considerations-and-limitations). MotherDuck preloads common DuckDB extensions but does not support loading custom extensions or user-defined functions.

### Attaching additional databases[​](#attaching-additional-databases "Direct link to Attaching additional databases")

DuckDB supports [attaching additional databases](https://duckdb.org/docs/sql/statements/attach.html) so you can read and write from multiple databases. Configure additional databases using the `attach` argument in your profile:

profiles.yml

```yaml
default:
  outputs:
    dev:
      type: duckdb
      path: /tmp/dbt.duckdb
      attach:
        - path: /tmp/other.duckdb
        - path: ./yet/another.duckdb
          alias: yet_another
        - path: s3://yep/even/this/works.duckdb
          read_only: true
        - path: sqlite.db
          type: sqlite
        - path: postgresql://username@hostname/dbname
          type: postgres
  target: dev
```

You can refer to attached databases by the basename of the file (without its suffix) or by an `alias` you specify. The `type` argument supports `duckdb`, `sqlite`, and `postgres`. You can also pass arbitrary options using the `options` dictionary — refer to [Arbitrary ATTACH options](https://docs.getdbt.com/reference/resource-configs/duckdb-configs.md#arbitrary-attach-options) for details.

For DuckLake, use `ducklake:` for local databases. For MotherDuck-managed DuckLake, use `md:` with `is_ducklake: true`. Refer to the [DuckLake configuration](https://docs.getdbt.com/reference/resource-configs/duckdb-configs.md#ducklake) section for details.

## Extensions[​](#extensions "Direct link to Extensions")

You can load any supported [DuckDB extensions](https://duckdb.org/docs/extensions/overview) by listing them in the `extensions` field in your profile. You can also set any additional [DuckDB configuration options](https://duckdb.org/docs/sql/configuration) in the `settings` field.

profiles.yml

```yaml
your_profile_name:
  target: dev
  outputs:
    dev:
      type: duckdb
      path: 'file_path/database_name.duckdb'
      extensions:
        - httpfs
        - parquet
      settings:
        s3_region: my-aws-region
        s3_access_key_id: "{{ env_var('S3_ACCESS_KEY_ID') }}"
        s3_secret_access_key: "{{ env_var('S3_SECRET_ACCESS_KEY') }}"
```

You can also configure extensions from outside the core extension repository (such as a community extension) by specifying a `name`/`repo` pair:

```yml
extensions:
  - httpfs
  - parquet
  - name: h3
    repo: community
  - name: uc_catalog
    repo: core_nightly
```

For configuring cloud storage access using DuckDB's Secrets Manager or fsspec filesystems, refer to the [DuckDB configurations](https://docs.getdbt.com/reference/resource-configs/duckdb-configs.md) page.

## More information[​](#more-information "Direct link to More information")

Find DuckDB-specific configuration information in the [DuckDB adapter reference guide](https://docs.getdbt.com/reference/resource-configs/duckdb-configs.md).

For adapter source code, refer to the [`dbt-duckdb` repository](https://github.com/duckdb/dbt-duckdb). For adapter release notes, refer to the [`dbt-duckdb` releases page](https://github.com/duckdb/dbt-duckdb/releases).
