# Connect Firebird to dbt v1

Local development

Community plugin

Some core functionality may be limited. If you're interested in contributing, check out the source code for each repository listed below.

* **Maintained by**: Community
* **Authors**: Fernando Lozer (https\://github.com/flozer)
* **GitHub repo**: [flozer/dbt-firebird](https://github.com/flozer/dbt-firebird) [![](https://img.shields.io/github/stars/flozer/dbt-firebird?style=for-the-badge)](https://github.com/flozer/dbt-firebird)
* **PyPI package**: `dbt-firebird` [![](https://badge.fury.io/py/dbt-firebird.svg)](https://badge.fury.io/py/dbt-firebird)
* **Slack channel**: [n/a](https://www.getdbt.com/community)
* **Supported dbt version**: v1.8.0 and newer
* **dbt support**: Not Supported
* **Minimum data platform version**: Firebird 3.0

## Installing dbt-firebird

Note: Installing an adapter doesn't install dbt v1 automatically. This is because adapters and dbt v1 versions are decoupled to avoid overwriting dbt v1 installations. Use the following command for installation:

`python -m pip install dbt-core dbt-firebird`

## Configuring dbt-firebird

For Firebird-specific configuration, please refer to [Firebird configs.](../../../reference/resource-configs/no-configs.md)

The [dbt-firebird](https://pypi.org/project/dbt-firebird/) adapter connects dbt v1 to [Firebird](https://firebirdsql.org/) databases (3.0, 4.0, and 5.0) through the official [firebird-driver](https://pypi.org/project/firebird-driver/) package. It supports table, view, incremental, seed, and snapshot (SCD2) materializations, data tests, generated documentation, and an interactive `dbt init` experience.

The adapter is validated against the official dbt v1 test suites and a CI matrix covering Firebird 3/4/5 on Python 3.9 to 3.12.

## Connecting to Firebird with dbt-firebird

Firebird databases are single files. The `path` field is the database file path as seen by the server. If dbt runs on the same machine as the server, this is a normal local path; for a remote server, it is a server-side path (for example, `/data/prod.fdb` on Linux).

### Local

Use this profile when Firebird is running on the same computer as dbt. You only need the database file `path`.

\~/.dbt/profiles.yml

```yaml
# Replace PROFILE_NAME with the profile name in your dbt_project.yml
PROFILE_NAME:
  target: dev
  outputs:
    dev:
      type: firebird
      path: C:/data/mydb.fdb
      user: SYSDBA
      password: masterkey
      threads: 1
```

### Remote server

Use this profile when Firebird is running on another computer. Include `host`, `port`, and the `path` as that server sees the file.

\~/.dbt/profiles.yml

```yaml
# Replace PROFILE_NAME with the profile name in your dbt_project.yml
PROFILE_NAME:
  target: dev
  outputs:
    dev:
      type: firebird
      host: 192.168.0.10
      port: 3050
      path: /data/prod.fdb
      user: SYSDBA
      password: masterkey
      charset: WIN1252
      threads: 1
```

### Description of Firebird profile fields

These fields apply to both local and remote profiles:

| Field         | Description                                                                                                                  | Required?                 | Example            |
| ------------- | ---------------------------------------------------------------------------------------------------------------------------- | ------------------------- | ------------------ |
| type          | The specific adapter to use                                                                                                  | Required                  | `firebird`         |
| path          | Database file path as seen by the server. Use forward slashes                                                                | Required                  | `C:/data/mydb.fdb` |
| host          | Server hostname or IP. Omit it to connect to a local file directly                                                           | Optional                  | `192.168.0.10`     |
| port          | Server TCP port                                                                                                              | Optional (default `3050`) | `3050`             |
| user          | Database user                                                                                                                | Required                  | `SYSDBA`           |
| password      | Password                                                                                                                     | Required                  | `masterkey`        |
| charset       | Connection character set. Refer to [Character sets](#character-sets)                                                         | Optional (default `UTF8`) | `WIN1252`          |
| role          | Firebird role, if you use them                                                                                               | Optional                  | `RDB$ADMIN`        |
| lock\_timeout | Seconds a statement waits for a lock held by another transaction before failing. `0` = do not wait, `-1` = wait indefinitely | Optional (default `10`)   | `30`               |
| threads       | Parallel model executions. `1` is recommended                                                                                | Optional (default `1`)    | `1`                |
| database      | Logical label used internally by dbt. Never rendered in SQL. Defaults to the file base name                                  | Optional                  | `mydb`             |
| schema        | Logical label used internally by dbt. Firebird has no schemas, so this is never rendered in SQL. Defaults to `main`          | Optional                  | `main`             |

`dbt init` also works: it lists `firebird`, asks for every field above, writes the profile, and runs `dbt debug` at the end.

## Character sets

Pick the `charset` value that matches how your Firebird database stores text:

| Situation                                                                                    | `charset`        |
| -------------------------------------------------------------------------------------------- | ---------------- |
| Database created with `UTF8`, or plain ASCII data                                            | `UTF8` (default) |
| Legacy/restored databases with character set `NONE` and accented data (for example, WIN1252) | `WIN1252`        |

If reads fail with `UnicodeDecodeError: 'utf-8' codec can't decode byte ...`, set `charset: WIN1252`. This is the most common case for legacy Brazilian Firebird databases.

## Object naming and case sensitivity

Unquoted identifiers in Firebird SQL are stored as UPPERCASE. The dbt adapter creates its objects with quoted, lowercase names (the same convention as the Postgres adapter), so a model named `clientes` becomes the table `"clientes"`:

```sql
select * from "clientes";  -- correct (quoted)
select * from clientes;    -- looks for CLIENTES, which does not exist
```

To expose objects in the unquoted/uppercase style, set a model or seed alias in uppercase:

```yaml
seeds:
  my_project:
    store:
      +alias: STORE
```

## Supported features

* Materializations: `table` (atomic swap), `view` (`CREATE OR ALTER`), `incremental` (strategies `append`, `delete+insert`, `merge`), `seed` (atomic data replacement), and `snapshot` (strategies `check` and `timestamp`)
* `on_schema_change` handling (`ignore`, `fail`, `append_new_columns`, `sync_all_columns`)
* Data tests, generic and singular
* `dbt docs generate` and `persist_docs` (writes `COMMENT ON` statements)
* Automatic retries for lock conflicts and a configurable `lock_timeout`

### Limitations

* The `ephemeral` materialization is not supported: dbt generates inline CTEs with names starting with an underscore, which are invalid as unquoted Firebird identifiers
* Firebird has no schemas: the `database` and `schema` profile fields are logical labels only
* Model and seed names should be at most 22 characters on Firebird 3 (dbt appends suffixes like `__dbt_tmp` to the 31-byte identifier limit; Firebird 4+ allows 63)
* Model contracts (`contract: enforced`) are best effort: declare Firebird-native data types in your YAML
* Models without a `FROM` clause are invalid in Firebird SQL (for example, use `select 1 as id from rdb$database`)
* `threads: 1` is recommended; the adapter is resilient to write conflicts, but Firebird applies table-level coordination across parallel writers in the same namespace

## Database privileges

The profile user needs read and write access to the objects dbt manages, plus permission to create and alter tables and views. These grants cover all dbt work: tables, views, seeds, snapshots, and incremental models:

| Privilege | Purpose                                                                                                                                                                |
| --------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `SELECT`  | Read tables and views                                                                                                                                                  |
| `INSERT`  | Load models, [seeds](../../build/seeds.md), and [snapshots](../../build/snapshots.md)                                    |
| `UPDATE`  | Change existing rows ([incremental models](../../build/incremental-models.md) and [snapshots](../../build/snapshots.md)) |
| `DELETE`  | Replace data (table swap and [incremental models](../../build/incremental-models.md))                                                           |
| `CREATE`  | Create tables and views                                                                                                                                                |
| `ALTER`   | Change tables and views                                                                                                                                                |
| `DROP`    | Replace objects dbt rebuilds                                                                                                                                           |

For SYSDBA or the database owner, no extra grants are needed.
