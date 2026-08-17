# Sail setup

* **Maintained by**: LakeSail
* **Authors**: LakeSail
* **GitHub repo**: [lakehq/dbt-sail](https://github.com/lakehq/dbt-sail) [![](https://img.shields.io/github/stars/lakehq/dbt-sail?style=for-the-badge)](https://github.com/lakehq/dbt-sail)
* **PyPI package**: `dbt-sail` [![](https://badge.fury.io/py/dbt-sail.svg)](https://badge.fury.io/py/dbt-sail)
* **Slack channel**: [LakeSail Community Slack](https://www.launchpass.com/lakesail-community/free)
* **Supported dbt Core version**: v1.8.0 and newer
* **dbt support**: Not Supported
* **Minimum data platform version**: n/a

## Installing dbt-sail

Use `pip` to install the adapter. Use the following command for installation:

`python -m pip install dbt-sail`

## Configuring dbt-sail

For Sail-specific configuration, please refer to [Sail configs.](../../../reference/resource-configs/spark-configs.md)

## Connecting to Sail with **dbt-sail**

[Sail](https://github.com/lakehq/sail) is a drop-in replacement for Apache Spark, built and maintained by [LakeSail](https://lakesail.com/). Sail implements the Spark Connect protocol, so `dbt-sail` is a thin wrapper around `dbt-spark` that connects to Sail instead of JVM Spark. `dbt-sail` supports the same feature set as `dbt-spark`.

`dbt-sail` supports two connection modes:

* `embedded` - starts a Sail server in-process via [`pysail`](https://pypi.org/project/pysail/). No external server required.
* `remote` - connects to an already-running Sail server via Spark Connect.

## Profile configuration

\~/.dbt/profiles.yml

```yaml
your_profile_name:
  target: dev
  outputs:
    dev:
      type: sail
      mode: embedded   # or 'remote'
      host: 127.0.0.1  # required for 'remote'; defaults to 127.0.0.1 for 'embedded'
      port: 50051      # used by 'remote'; ignored by 'embedded'
      schema: SCHEMA_NAME
      threads: 1
```

| Field                    | Description                                                       | Required?             | Default                |
| ------------------------ | ----------------------------------------------------------------- | --------------------- | ---------------------- |
| `type`                   | Must be `sail`.                                                   | Required              | -                      |
| `mode`                   | Connection mode. Either `embedded` or `remote`.                   | Required              | -                      |
| `host`                   | Hostname of the Sail server. Required for `remote` mode.          | Required for `remote` | `127.0.0.1` (embedded) |
| `port`                   | Spark Connect port on the Sail server. Used by `remote` mode.     | Optional              | `50051`                |
| `schema`                 | Default schema where dbt builds objects.                          | Required              | -                      |
| `threads`                | Number of threads for dbt to use.                                 | Optional              | `1`                    |
| `server_side_parameters` | Map of parameters passed through to the underlying Spark session. | Optional              | `{}`                   |

Because `dbt-sail` extends `dbt-spark`, additional `dbt-spark` profile fields are also accepted. See the [Apache Spark setup](./spark-setup.md) page for the full list.

### Example: embedded mode

\~/.dbt/profiles.yml

```yaml
my_sail_project:
  target: dev
  outputs:
    dev:
      type: sail
      mode: embedded
      schema: analytics
      threads: 4
```

### Example: remote mode

\~/.dbt/profiles.yml

```yaml
my_sail_project:
  target: dev
  outputs:
    dev:
      type: sail
      mode: remote
      host: sail.internal
      port: 50051
      schema: analytics
      threads: 4
```

## Resource configuration

`dbt-sail` accepts the same resource configurations as `dbt-spark`. See the [Apache Spark configurations](../../../reference/resource-configs/spark-configs.md) page for materialization options, incremental strategies, and other model-level configs.
