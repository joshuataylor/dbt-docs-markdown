# Snowflake configurations

Snowflake column size change

[Snowflake plans to increase](https://docs.snowflake.com/en/release-notes/bcr-bundles/un-bundled/bcr-2118) the default column size for string and binary data types in September 2026. `dbt-snowflake` versions below v1.10.6 may fail to build certain incremental models when this change is deployed.

 Assess impact and required actions

If you're using a `dbt-snowflake` version below v1.10.6 or have not yet migrated to a [release track](https://docs.getdbt.com/docs/dbt-versions/dbt-release-tracks.md) in the dbt platform, your adapter version is incompatible with this change and may fail to build incremental models that meet *both* of the following conditions:

* Contain string columns with collation defined
* Use the `on_schema_change='sync_all_columns'` config

To check whether this change affects your project, run the following [list](https://docs.getdbt.com/reference/commands/list.md) command:

```bash
dbt ls -s config.materialized:incremental,config.on_schema_change:sync_all_columns --resource-type model
```

* If the command returns `No nodes selected!`, no action is required.

* If the command returns one or more models (for example, `Found 1000 models, 644 macros`), you may be impacted if those models have string columns that don't specify a width. In that case, upgrade to a version that includes the fix:

  * **dbt Core**: `dbt-snowflake` v1.10.6 or later. For upgrade instructions, refer to [Upgrade adapters](https://docs.getdbt.com/docs/local/install-dbt.md) in the dbt Core v1 installation instructions.
  * **dbt platform**: Any release track (Latest, Compatible, Extended, or Fallback).
  * **dbt Fusion engine**: v2.0.0-preview\.147 or higher.

  This ensures your incremental models can safely handle schema changes while maintaining required collation settings.

## Iceberg table format[​](#iceberg-table-format "Direct link to Iceberg table format")

Our Snowflake Iceberg table content has moved to a [new page](https://docs.getdbt.com/docs/mesh/iceberg/snowflake-iceberg-support.md)!

## Dynamic tables[​](#dynamic-tables "Direct link to Dynamic tables")

The Snowflake adapter supports [dynamic tables](https://docs.snowflake.com/en/user-guide/dynamic-tables-about). This materialization is specific to Snowflake, which means that any model configuration that would normally come along for the ride from `dbt-core` (e.g. as with a `view`) may not be available for dynamic tables. This gap will decrease in future patches and versions. While this materialization is specific to Snowflake, it very much follows the implementation of [materialized views](https://docs.getdbt.com/docs/build/materializations.md#Materialized-View). In particular, dynamic tables have access to the `on_configuration_change` setting. Dynamic tables are supported with the following configuration parameters:

<!-- -->

<!-- -->

| Parameter                                                                                                  | Type                   | Required | Default     | Change Monitoring Support |
| ---------------------------------------------------------------------------------------------------------- | ---------------------- | -------- | ----------- | ------------------------- |
| [`on_configuration_change`](https://docs.getdbt.com/reference/resource-configs/on_configuration_change.md) | `<string>`             | no       | `apply`     | n/a                       |
| [`target_lag`](#target-lag)                                                                                | `<string>`             | no       |             | alter                     |
| [`scheduler`](#scheduler)                                                                                  | `<string>`             | no       | `DISABLE`   | alter                     |
| [`snowflake_warehouse`](#configuring-virtual-warehouses)                                                   | `<string>`             | yes      |             | alter                     |
| [`snowflake_initialization_warehouse`](#initialization-warehouse)                                          | `<string>`             | no       | `None`      | alter                     |
| [`refresh_warehouse`](#refresh-warehouse)                                                                  | `<string>`             | no       | `None`      | alter                     |
| [`refresh_mode`](#refresh-mode)                                                                            | `<string>`             | no       | `AUTO`      | refresh                   |
| [`initialize`](#initialize)                                                                                | `<string>`             | no       | `ON_CREATE` | n/a                       |
| [`cluster_by`](#dynamic-table-clustering)                                                                  | `<string>` or `<list>` | no       | `None`      | alter                     |
| [`immutable_where`](#immutable-where)                                                                      | `<string>`             | no       | `None`      | alter                     |
| [`copy_grants`](#copy-grants-dynamic-tables)                                                               | `<boolean>`            | no       | `false`     | full refresh              |
| [`transient`](#transient-dynamic-tables)                                                                   | `<boolean>`            | no       | `false`     | full refresh              |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

* Project YAML file
* Properties YAML file
* SQL file config

dbt\_project.yml

```yaml
models:
  <resource-path>:
    +materialized: dynamic_table
    +on_configuration_change: apply | continue | fail
    +target_lag: downstream | <time-delta>
    +scheduler: ENABLE | DISABLE
    +snowflake_warehouse: <warehouse-name>
    +snowflake_initialization_warehouse: <warehouse-name>
    +refresh_warehouse: <warehouse-name>
    +refresh_mode: AUTO | FULL | INCREMENTAL
    +initialize: ON_CREATE | ON_SCHEDULE 
    +cluster_by: <column-name> | [<column-name>, <column-name>, ...]
    +immutable_where: <condition>
    +copy_grants: true | false
    +transient: true | false
```

models/properties.yml

```yaml

models:
  - name: [<model-name>]
    config:
      materialized: dynamic_table
      on_configuration_change: apply | continue | fail
      target_lag: downstream | <time-delta>
      scheduler: ENABLE | DISABLE
      snowflake_warehouse: <warehouse-name>
      snowflake_initialization_warehouse: <warehouse-name>
      refresh_warehouse: <warehouse-name>
      refresh_mode: AUTO | FULL | INCREMENTAL
      initialize: ON_CREATE | ON_SCHEDULE 
      cluster_by: <column-name> | [<column-name>, <column-name>, ...]
      immutable_where: <condition>
      copy_grants: true | false
      transient: true | false
```

models/\<model\_name>.sql

```jinja

{{ config(
    materialized="dynamic_table",
    on_configuration_change="apply" | "continue" | "fail",
    target_lag="downstream" | "<integer> seconds | minutes | hours | days",
    scheduler="ENABLE" | "DISABLE",
    snowflake_warehouse="<warehouse-name>",
    snowflake_initialization_warehouse="<warehouse-name>",
    refresh_warehouse="<warehouse-name>",
    refresh_mode="AUTO" | "FULL" | "INCREMENTAL",
    initialize="ON_CREATE" | "ON_SCHEDULE", 
    cluster_by="<column-name>" | ["<column-name>", "<column-name>", ...],
    immutable_where="<condition>",
    copy_grants=true | false,
    transient=true | false,

) }}
```

Learn more about these parameters in Snowflake's [docs](https://docs.snowflake.com/en/sql-reference/sql/create-dynamic-table):

### Target lag[​](#target-lag "Direct link to Target lag")

Snowflake allows two configuration scenarios for scheduling automatic refreshes:

* **Time-based** — Provide a value of the form `<int> { seconds | minutes | hours | days }`. For example, if the dynamic table needs to be updated every 30 minutes, use `target_lag='30 minutes'`.
* **Downstream** — Applicable when the dynamic table is referenced by other dynamic tables. In this scenario, `target_lag='downstream'` allows for refreshes to be controlled at the target, instead of at each layer.

#### How `target_lag` interacts with `scheduler`[​](#how-target_lag-interacts-with-scheduler "Direct link to how-target_lag-interacts-with-scheduler")

`target_lag` works with [`scheduler`](#scheduler) to determine how dynamic table refreshes are managed:

| `target_lag` | `scheduler`          | Behavior                                                                                            |
| ------------ | -------------------- | --------------------------------------------------------------------------------------------------- |
| Set          | `ENABLE` or omitted  | Snowflake manages refreshes automatically. If `scheduler` is omitted, dbt defaults to `ENABLE`.     |
| Not set      | `DISABLE` or omitted | dbt manages refreshes during model execution. If `scheduler` is omitted, dbt defaults to `DISABLE`. |
| Set          | `DISABLE`            | Invalid: `DISABLE` does not accept `target_lag`. dbt raises an error.                               |
| Not set      | `ENABLE`             | Invalid: `ENABLE` requires `target_lag`. dbt raises an error.                                       |

Learn more about `target_lag` in Snowflake's [docs](https://docs.snowflake.com/en/user-guide/dynamic-tables-refresh#understanding-target-lag). Please note that Snowflake supports a target lag of 1 minute or longer.

### Scheduler[​](#scheduler "Direct link to Scheduler")

The `scheduler` parameter controls whether the dynamic table is refreshed by Snowflake's background scheduler or by an external orchestrator (for example, dbt). Snowflake accepts two options:

* **ENABLE** — Snowflake's built-in scheduler automatically refreshes the dynamic table based on the defined `target_lag`. Refreshes cascade across the dependency graph to maintain snapshot consistency. Setting `target_lag` is *required* when using this option.
* **DISABLE** — The dynamic table is excluded from Snowflake's automatic background refresh. You must trigger refreshes manually or through orchestration external to Snowflake (for example, by a `dbt run` that executes `ALTER DYNAMIC TABLE ... REFRESH`). When this option is explicitly set, specifying `target_lag` results in an error.

dbt default differs from Snowflake's native default

In Snowflake's native DDL, omitting `SCHEDULER` defaults to `ENABLE`, and `TARGET_LAG` is required.

In dbt, the default value is `DISABLE`. If neither `scheduler` nor `target_lag` is specified, dbt creates the dynamic table with `scheduler: DISABLE` and manages refreshes directly.

If you specify `target_lag` without explicitly setting `scheduler`, dbt sets `scheduler: ENABLE`.

**Key points:**

* Explicitly setting `scheduler: DISABLE` together with `target_lag` results in an error. If you omit `scheduler` and provide `target_lag`, dbt resolves the conflict by setting `scheduler: ENABLE` automatically.
* When `scheduler: DISABLE`, a manual refresh does *not* automatically refresh upstream dynamic table dependencies. This creates an isolation boundary, allowing dbt to manage specific table refreshes without triggering the entire pipeline. In contrast, `ENABLE` cascades refreshes across the dependency graph.
* If a dynamic table with `scheduler: DISABLE` depends on other dynamic tables, those upstream tables will not be refreshed when the downstream table is refreshed. dbt must manage the refresh order explicitly.

For example, to let dbt manage refreshes (default behavior):

```sql
{{ config(
    materialized='dynamic_table',
    snowflake_warehouse='MY_WH',
) }}

select * from {{ source('raw', 'events') }}
```

To enable Snowflake-managed scheduling with a target lag:

```sql
{{ config(
    materialized='dynamic_table',
    snowflake_warehouse='MY_WH',
    target_lag='5 minutes',
) }}

select * from {{ source('raw', 'events') }}
```

Learn more about `scheduler` in [Snowflake's docs](https://docs.snowflake.com/en/sql-reference/sql/create-dynamic-table#optional-parameters).

### Refresh warehouse[​](#refresh-warehouse "Direct link to Refresh warehouse")

Starting `dbt-snowflake` v1.11, you can use the `refresh_warehouse` parameter in your model configuration to specify a separate warehouse for the dynamic table's self-refresh operations. This is separate from [`snowflake_warehouse`](#configuring-virtual-warehouses), which controls DDL execution. By setting `refresh_warehouse`, you can use a smaller warehouse for automatic refreshes while keeping a larger `snowflake_warehouse` for DDL operations.

To configure the `refresh_warehouse` parameter in your model, refer to the following example:

models/\<model\_name>.sql

```sql
{{ config(
    materialized='dynamic_table',
    snowflake_warehouse='LARGE_EXECUTION_WH',
    refresh_warehouse='SMALL_REFRESH_WH',
    target_lag='1 hour'
) }}

select * from {{ source('raw', 'events') }}
```

**Key points:**

* If `refresh_warehouse` is not set, `snowflake_warehouse` is used for both DDL execution and self-refresh operations.
* You can change `refresh_warehouse` on an existing dynamic table without a full refresh.
* To revert to the default behavior after setting a refresh warehouse, remove the `refresh_warehouse` parameter from your model configuration or explicitly set it to `None`.

Learn more about the `WAREHOUSE` parameter in [Snowflake's docs](https://docs.snowflake.com/en/user-guide/dynamic-tables-warehouses).

### Refresh mode[​](#refresh-mode "Direct link to Refresh mode")

Snowflake allows three options for refresh mode:

* **AUTO** — Enforces an incremental refresh of the dynamic table by default. If the `CREATE DYNAMIC TABLE` statement does not support the incremental refresh mode, the dynamic table is automatically created with the full refresh mode.
* **FULL** — Enforces a full refresh of the dynamic table, even if the dynamic table can be incrementally refreshed.
* **INCREMENTAL** — Enforces an incremental refresh of the dynamic table. If the query that underlies the dynamic table can’t perform an incremental refresh, dynamic table creation fails and displays an error message.

Learn more about `refresh_mode` in [Snowflake's docs](https://docs.snowflake.com/en/user-guide/dynamic-tables-refresh).

### Initialize[​](#initialize "Direct link to Initialize")

Snowflake allows two options for initialize:

* **ON\_CREATE** — Refreshes the dynamic table synchronously at creation. If this refresh fails, dynamic table creation fails and displays an error message.
* **ON\_SCHEDULE** — Refreshes the dynamic table at the next scheduled refresh.

Learn more about `initialize` in [Snowflake's docs](https://docs.snowflake.com/en/user-guide/dynamic-tables-refresh).

### Immutable where[​](#immutable-where "Direct link to Immutable where")

Snowflake allows you to mark certain rows of a dynamic table as immutable using the `IMMUTABLE WHERE` clause. This prevents Snowflake from applying updates or deletions to matching rows during refreshes, so historical data stays the same and refreshes run faster.

From dbt Core v1.11, you can configure this using the `immutable_where` configuration. This config accepts a SQL condition expression and rows that match it are treated as immutable and won’t be updated or deleted during future refreshes.

For example, to mark data older than 1 day as immutable since historical data typically doesn't change:

```sql
{{ config(
    materialized='dynamic_table',
    snowflake_warehouse='MY_WH',
    target_lag='1 hour',
    immutable_where='ts < CURRENT_TIMESTAMP() - INTERVAL \'1 DAY\''
) }}

select
    id,
    ts,
    value
from {{ source('raw', 'events') }}
```

**Key points:**

* The config supports Jinja rendering (for example, dbt variables and macros), as long as the rendered result is a valid Snowflake SQL condition.
* To remove the immutable constraint from an existing dynamic table, set `immutable_where` to `None`.
* You can alter changes to `immutable_where` without a full refresh.

Learn more about `IMMUTABLE WHERE` in [Snowflake's docs](https://docs.snowflake.com/en/user-guide/dynamic-tables-immutability-constraints).

### Copy grants (dynamic tables)[​](#copy-grants-dynamic-tables "Direct link to Copy grants (dynamic tables)")

Starting `dbt-snowflake` v1.11, you can use `copy_grants` to preserve existing object-level privileges when dbt generates a `CREATE OR REPLACE DYNAMIC TABLE` statement. When disabled, all previously granted permissions are dropped when the table is recreated, and downstream users or roles lose access until grants are manually re-applied.

When you set `copy_grants: true` on a dynamic table, dbt adds the `COPY GRANTS` clause to the `CREATE OR REPLACE DYNAMIC TABLE` statement. This preserves existing object-level privileges on the table during `--full-refresh` runs, so you don't need to re-grant access after the table is recreated.

To configure the `copy_grants` parameter, refer to the following example:

```sql
{{ config(
    materialized='dynamic_table',
    snowflake_warehouse='MY_WH',
    target_lag='1 hour',
    copy_grants=true
) }}

select * from {{ source('raw', 'events') }}
```

Learn more about `COPY GRANTS` in [Snowflake's docs](https://docs.snowflake.com/en/sql-reference/sql/create-dynamic-table).

### Transient (dynamic tables)[​](#transient-dynamic-tables "Direct link to Transient (dynamic tables)")

You can create dynamic tables as transient to reduce storage costs. Transient dynamic tables do not use Snowflake's [Fail-safe](https://docs.snowflake.com/en/user-guide/data-failsafe) period, so they consume less storage than permanent dynamic tables. To create a dynamic table as transient, set `transient: true` in the model configuration.

If you want all dynamic tables to be transient by default (without setting `transient: true` on each one), enable the [`snowflake_default_transient_dynamic_tables`](https://docs.getdbt.com/reference/global-configs/snowflake-changes.md#the-snowflake_default_transient_dynamic_tables-flag) flag in your `dbt_project.yml`. This flag defaults to `false`, meaning dynamic tables are created as permanent by default.

**Key points:**

* Setting `transient: true` creates the dynamic table with the `TRANSIENT` keyword in the `CREATE DYNAMIC TABLE` statement.
* Snowflake does not support changing the transient property on an existing dynamic table. Changing `transient` from `true` to `false` or vice versa triggers a full table recreation.
* To make all new dynamic tables transient by default when `transient` is not specified, enable the `snowflake_default_transient_dynamic_tables` flag in your `dbt_project.yml`.

For example:

```sql
{{ config(
    materialized='dynamic_table',
    snowflake_warehouse='MY_WH',
    target_lag='1 hour',
    transient=true
) }}

select * from {{ source('raw', 'events') }}
```

### Initialization warehouse[​](#initialization-warehouse "Direct link to Initialization warehouse")

Snowflake supports an `INITIALIZATION_WAREHOUSE` parameter that specifies which virtual warehouse to use when initializing or reinitializing a dynamic table.

Starting `dbt-snowflake` v1.12, you can use the `snowflake_initialization_warehouse` parameter to configure this. This is separate from the `snowflake_warehouse` parameter used for regular incremental refreshes. By setting `snowflake_initialization_warehouse`, you can use a larger warehouse for the initial build and reinitialization, while keeping `snowflake_warehouse` smaller for regular refreshes.

To configure the `snowflake_initialization_warehouse` parameter, refer to the following example:

```sql
{{ config(
    materialized='dynamic_table',
    snowflake_warehouse='COMPUTE_WH',
    snowflake_initialization_warehouse='LARGE_WH',
    target_lag='1 minute'
) }}

select * from {{ source('raw', 'events') }}
```

**Key points:**

* If `snowflake_initialization_warehouse` is not set, Snowflake uses `snowflake_warehouse` for both initialization and regular refreshes.
* You can change `snowflake_initialization_warehouse` on an existing dynamic table without a full refresh.
* To revert to the default behavior after setting an initialization warehouse, either remove the `snowflake_initialization_warehouse` parameter from your model configuration or explicitly set it to `None`.

Learn more about `INITIALIZATION_WAREHOUSE` in [Snowflake's docs](https://docs.snowflake.com/en/user-guide/dynamic-tables-warehouses).

### Limitations[​](#limitations "Direct link to Limitations")

As with materialized views on most data platforms, there are limitations associated with dynamic tables. Some worth noting include:

* Dynamic table SQL has a [limited feature set](https://docs.snowflake.com/en/user-guide/dynamic-tables-tasks-create#query-constructs-not-currently-supported-in-dynamic-tables).
* Dynamic table SQL cannot be updated; the dynamic table must go through a `--full-refresh` (DROP/CREATE).
* Dynamic tables cannot be downstream from: materialized views, external tables, streams.
* Dynamic tables cannot reference a view that is downstream from another dynamic table.

Find more information about dynamic table limitations in Snowflake's [docs](https://docs.snowflake.com/en/user-guide/dynamic-tables-tasks-create#dynamic-table-limitations-and-supported-functions).

For dbt limitations, [Model contracts](https://docs.getdbt.com/docs/mesh/govern/model-contracts.md) are not supported.

### Troubleshooting dynamic tables[​](#troubleshooting-dynamic-tables "Direct link to Troubleshooting dynamic tables")

If your dynamic table model fails to rerun with the following error message after the initial execution:

```sql
SnowflakeDynamicTableConfig.__init__() missing 6 required positional arguments: 'name', 'schema_name', 'database_name', 'query', 'target_lag', and 'snowflake_warehouse'
```

Ensure that `QUOTED_IDENTIFIERS_IGNORE_CASE` on your account is set to `FALSE`.

## Semantic Views[​](#semantic-views "Direct link to Semantic Views")

[Snowflake Semantic Views](https://docs.snowflake.com/en/user-guide/views-semantic/overview) provide a native schema-level object for centralizing metric definitions and reducing fragmented metric logic across BI and analytics tools.

Use the [`dbt_semantic_view` package](https://hub.getdbt.com/Snowflake-Labs/dbt_semantic_view/latest/) to define and manage Snowflake Semantic Views in your dbt project. This lets you keep Semantic View definitions in version control and apply your existing testing and CI/CD workflows to your Semantic Layer.

### Install the package[​](#install-the-package "Direct link to Install the package")

Prerequisite

* This package requires `dbt` version `>=1.0.0, <2.0.0`. For the latest compatibility details, refer to the [`dbt_semantic_view` package](https://hub.getdbt.com/Snowflake-Labs/dbt_semantic_view/latest/).
* Your Snowflake account supports Semantic Views.
* Your role has permission to create Semantic Views.
* You can write to a database and schema where you have create privileges.

Add `dbt_semantic_view` to your `packages.yml` file:

```yaml
packages:
  - package: Snowflake-Labs/dbt_semantic_view
    version: 1.0.3
```

Run `dbt deps` to install package dependencies:

```shell
dbt deps
```

Verify the package was installed by confirming `dbt_semantic_view` is present in your `dbt_packages/` directory.

### Highlighted features[​](#highlighted-features "Direct link to Highlighted features")

The `dbt_semantic_view` package includes the following features for defining and managing Snowflake Semantic Views in dbt projects.

#### Materialize models as Snowflake Semantic Views[​](#materialize-models-as-snowflake-semantic-views "Direct link to Materialize models as Snowflake Semantic Views")

Use the `semantic_view` materialization to define Snowflake Semantic Views in dbt, including tables, relationships, facts, dimensions, and metrics.

Semantic view models use Snowflake’s semantic view syntax (for example, `TABLES`, `DIMENSIONS`, and `METRICS`) rather than a standard `SELECT` query.

The example below is adapted from [Getting Started with Snowflake Semantic View](https://quickstarts.snowflake.com/guide/snowflake-semantic-view/index.html?index=..%2F..index#3).

```sql
{{ config(materialized='semantic_view') }}

tables (
    CUSTOMER as {{ SOURCE('<SOURCE_NAME>', 'CUSTOMER') }} primary key (C_CUSTOMER_SK),
    DATE as {{ SOURCE('<SOURCE_NAME>', 'DATE_DIM') }} primary key (D_DATE_SK),
    DEMO as {{ SOURCE('<SOURCE_NAME>', 'CUSTOMER_DEMOGRAPHICS') }} primary key (CD_DEMO_SK),
    ITEM as {{ SOURCE('<SOURCE_NAME>', 'ITEM') }} primary key (I_ITEM_SK),
    STORE as {{ SOURCE('<SOURCE_NAME>', 'STORE') }} primary key (S_STORE_SK),
    STORESALES as {{ SOURCE('<SOURCE_NAME>', 'STORESALES') }}
    primary key (SS_SOLD_DATE_SK,SS_CDEMO_SK,SS_ITEM_SK,SS_STORE_SK,SS_CUSTOMER_SK)
)
relationships (
    SALESTOCUSTOMER as STORESALES(SS_CUSTOMER_SK) references CUSTOMER(C_CUSTOMER_SK),
    SALESTODATE as STORESALES(SS_SOLD_DATE_SK) references DATE(D_DATE_SK),
    SALESTODEMO as STORESALES(SS_CDEMO_SK) references DEMO(CD_DEMO_SK),
    SALESTOITEM as STORESALES(SS_ITEM_SK) references ITEM(I_ITEM_SK),
    SALETOSTORE as STORESALES(SS_STORE_SK) references STORE(S_STORE_SK)
)
facts (
    ITEM.COST as i_wholesale_cost,
    ITEM.PRICE as i_current_price,
    STORE.TAX_RATE as S_TAX_PERCENTAGE,
    STORESALES.SALES_QUANTITY as SS_QUANTITY
)
dimensions (
    CUSTOMER.BIRTHYEAR as C_BIRTH_YEAR,
    CUSTOMER.COUNTRY as C_BIRTH_COUNTRY,
    CUSTOMER.C_CUSTOMER_SK as c_customer_sk,
    DATE.DATE as D_DATE,
    DATE.D_DATE_SK as d_date_sk,
    DATE.MONTH as D_MOY,
    DATE.WEEK as D_WEEK_SEQ,
    DATE.YEAR as D_YEAR,
    DEMO.CD_DEMO_SK as cd_demo_sk,
    DEMO.CREDIT_RATING as CD_CREDIT_RATING,
    DEMO.MARITAL_STATUS as CD_MARITAL_STATUS,
    ITEM.BRAND as I_BRAND,
    ITEM.CATEGORY as I_CATEGORY,
    ITEM.CLASS as I_CLASS,
    ITEM.I_ITEM_SK as i_item_sk,
    STORE.MARKET as S_MARKET_ID,
    STORE.SQUAREFOOTAGE as S_FLOOR_SPACE,
    STORE.STATE as S_STATE,
    STORE.STORECOUNTRY as S_COUNTRY,
    STORE.S_STORE_SK as s_store_sk,
    STORESALES.SS_CDEMO_SK as ss_cdemo_sk,
    STORESALES.SS_CUSTOMER_SK as ss_customer_sk,
    STORESALES.SS_ITEM_SK as ss_item_sk,
    STORESALES.SS_SOLD_DATE_SK as ss_sold_date_sk,
    STORESALES.SS_STORE_SK as ss_store_sk
)
metrics (
    STORESALES.TOTALCOST as SUM(item.cost),
    STORESALES.TOTALSALESPRICE as SUM(SS_SALES_PRICE),
    STORESALES.TOTALSALESQUANTITY as SUM(SS_QUANTITY)
        WITH SYNONYMS = ('total sales quantity', 'total sales amount')
)
```

When you run dbt, this model compiles to a Snowflake `CREATE SEMANTIC VIEW` statement.

#### Reference Semantic Views in other dbt models[​](#reference-semantic-views-in-other-dbt-models "Direct link to Reference Semantic Views in other dbt models")

Use `ref()` for Semantic Views defined in your dbt project, and use `source()` for existing external Semantic Views.

```sql
{{ config(materialized='view') }}

select * from semantic_view(
  {{ ref('<semantic_view_model_name>') }}
  METRICS ...
  DIMENSIONS ...
  WHERE ...
)
```

```sql
{{ config(materialized='table') }}

select * from semantic_view(
  {{ source('<source_name>', '<semantic_view>') }}
  METRICS ...
  DIMENSIONS ...
  WHERE ...
)
```

## Temporary tables[​](#temporary-tables "Direct link to Temporary tables")

To save compile time and avoid the database write step initiated by a temporary table, incremental table merges for Snowflake prefer to utilize a `view` rather than a `temporary table` .

Sometimes a temporary table achieves results faster or more safely. You can opt in to temporary or transient tables for incremental builds by using the `tmp_relation_type` configuration This is defined as part of the model configuration.

To guarantee accuracy, an incremental model using the `delete+insert` strategy with a `unique_key` defined requires a temporary table; trying to change this to a view will result in an error.

`tmp_relation_type` accepts these values:

* `view` (default): Skips intermediate step of creating a temporary physical table for the tmp relation; fastest but not suitable for all strategies.
* `table`: A session-scoped temporary table; not visible in the Snowflake catalog and isolated per session.
* `transient`: A transient table; persists in the catalog, enabling [Snowflake native lineage tracking](https://docs.snowflake.com/en/user-guide/ui-snowsight-lineage), while avoiding the 7-day fail-safe storage costs of permanent tables. **Note:** This value is distinct from the separate model-level `transient` config described later, which controls the final model relation.

Defined in the project YAML:

dbt\_project.yml

```yaml
name: my_project

...

models:
  <resource-path>:
    +tmp_relation_type: table | view | transient ## If not defined, view is the default.
  
```

In the configuration format for the model SQL file:

dbt\_model.sql

```sql

{{ config(
    tmp_relation_type="table | view | transient", 
    -- If not defined, view is the default.
) }}
```

Concurrent run conflicts with `transient`

When `tmp_relation_type` is set to `transient`, the tmp relation becomes a real table that persists in the target schema under a deterministic name. If multiple runs of the same incremental model execute concurrently in the same schema, they can overwrite each other's tmp relation, causing data duplication or incorrect results. For example, this might happen when developers share a target schema or when CI and production runs overlap.

This risk depends on how you configure schemas and databases for your dbt models. To prevent conflicts, use `snowflake__resolve_incremental_tmp_relation` to route tmp relations to a schema that is unique per run or environment. For more information, refer to [Avoiding tmp relation conflicts](#avoiding-tmp-relation-conflicts).

### Avoiding tmp relation conflicts[​](#avoiding-tmp-relation-conflicts "Direct link to Avoiding tmp relation conflicts")

To prevent name collisions across concurrent runs, override the `snowflake__resolve_incremental_tmp_relation` dispatch macro to redirect the tmp relation to a dedicated schema:

macros/snowflake\_incremental.sql

```sql
{% macro snowflake__resolve_incremental_tmp_relation(tmp_relation) %}
  {{ return(tmp_relation.incorporate(schema='scratch')) }}
{% endmacro %}
```

This macro receives the default tmp relation object and returns a modified version. Common overrides include appending a developer username, a CI job ID, or a target name to the schema to ensure isolation across concurrent runs.

To append a target name to the schema:

macros/snowflake\_incremental.sql

```sql
{% macro snowflake__resolve_incremental_tmp_relation(tmp_relation) %}
  {%- set scratch_schema = target.schema ~ '_scratch_' ~ env_var('DBT_JOB_ID', target.name) -%}
  {{ return(tmp_relation.incorporate(schema=scratch_schema)) }}
{% endmacro %}
```

## Transient tables[​](#transient-tables "Direct link to Transient tables")

Snowflake supports the creation of [transient tables](https://docs.snowflake.net/manuals/user-guide/tables-temp-transient.html). Snowflake does not preserve a history for these tables, which can result in a measurable reduction of your Snowflake storage costs. Transient tables participate in time travel to a limited degree with a retention period of 1 day by default with no fail-safe period. Weigh these tradeoffs when deciding whether or not to configure your dbt models as `transient`. **By default, all Snowflake tables created by dbt are `transient`.**

### Configuring transient tables in dbt\_project.yml[​](#configuring-transient-tables-in-dbt_projectyml "Direct link to Configuring transient tables in dbt_project.yml")

A whole folder (or package) can be configured to be transient (or not) by adding a line to the `dbt_project.yml` file. This config works just like all of the [model configs](https://docs.getdbt.com/reference/model-configs.md) defined in `dbt_project.yml`.

dbt\_project.yml

```yaml
name: my_project

...

models:
  +transient: false
  my_project:
    ...
```

### Configuring transience for a specific model[​](#configuring-transience-for-a-specific-model "Direct link to Configuring transience for a specific model")

A specific model can be configured to be transient by setting the `transient` model config to `true`.

my\_table.sql

```sql
{{ config(materialized='table', transient=true) }}

select * from ...
```

## Query tags[​](#query-tags "Direct link to Query tags")

[Query tags](https://docs.snowflake.com/en/sql-reference/parameters.html#query-tag) are a Snowflake parameter that can be quite useful later on when searching in the [QUERY\_HISTORY view](https://docs.snowflake.com/en/sql-reference/account-usage/query_history.html).

dbt supports setting a default query tag for the duration of its Snowflake connections in [your profile](https://docs.getdbt.com/docs/local/connect-data-platform/snowflake-setup.md). You can set more precise values (and override the default) for subsets of models by setting a `query_tag` model config or by overriding the default `set_query_tag` macro:

dbt\_project.yml

```yaml
models:
  <resource-path>:
    +query_tag: dbt_special
```

models/\<modelname>.sql

```sql
{{ config(
    query_tag = 'dbt_special'
) }}

select ...
```

In this example, you can set up a query tag to be applied to every query with the model's name.

```sql

  {% macro set_query_tag() -%}
  {% set new_query_tag = model.name %} 
  {% if new_query_tag %}
    {% set original_query_tag = get_current_query_tag() %}
    {{ log("Setting query_tag to '" ~ new_query_tag ~ "'. Will reset to '" ~ original_query_tag ~ "' after materialization.") }}
    {% do run_query("alter session set query_tag = '{}'".format(new_query_tag)) %}
    {{ return(original_query_tag)}}
  {% endif %}
  {{ return(none)}}
{% endmacro %}
```

**Note:** query tags are set at the *session* level. At the start of each model materialization, if the model has a custom `query_tag` configured, dbt will run `alter session set query_tag` to set the new value. At the end of the materialization, dbt will run another `alter` statement to reset the tag to its default value. As such, build failures midway through a materialization may result in subsequent queries running with an incorrect tag.

## Merge behavior (incremental models)[​](#merge-behavior-incremental-models "Direct link to Merge behavior (incremental models)")

The [`incremental_strategy` config](https://docs.getdbt.com/docs/build/incremental-strategy.md) controls how dbt builds incremental models. By default, dbt will use a [merge statement](https://docs.snowflake.net/manuals/sql-reference/sql/merge.html) on Snowflake to refresh incremental tables.

Snowflake supports the following incremental strategies:

* [`merge`](https://docs.getdbt.com/docs/build/incremental-strategy.md#merge) (default)
* [`append`](https://docs.getdbt.com/docs/build/incremental-strategy.md#append)
* [`delete+insert`](https://docs.getdbt.com/docs/build/incremental-strategy.md#deleteinsert)
* [`insert_overwrite`](https://docs.getdbt.com/docs/build/incremental-strategy.md#insert_overwrite)
  * Note: This is not a standard dbt incremental strategy. `insert_overwrite` behaves like `truncate` + re-`insert` commands on Snowflake. It doesn't support partition-based overwrites, which means it'll overwrite the entire table intentionally. It's implemented as an incremental strategy because it aligns with dbt's workflow of not dropping existing tables. You can use [`overwrite_columns`](#overwrite_columns) to control which columns are included in the `INSERT OVERWRITE` statement.
* [`microbatch`](https://docs.getdbt.com/docs/build/incremental-microbatch.md)

Snowflake's `merge` statement fails with a "nondeterministic merge" error if the `unique_key` specified in your model config is not actually unique. If you encounter this error, you can instruct dbt to use a two-step incremental approach by setting the `incremental_strategy` config for your model to `delete+insert`.

### `overwrite_columns`[​](#overwrite_columns "Direct link to overwrite_columns")

When using `incremental_strategy='insert_overwrite'` on Snowflake, you can set `overwrite_columns` to control how dbt generates the `INSERT OVERWRITE` statement for your incremental model. For example:

models/my\_model.sql

```sql
{{ config(
    materialized='incremental',
    incremental_strategy='insert_overwrite',
    overwrite_columns=['id', 'value', 'event_date']
) }}

select id, value, event_date
from {{ ref('my_source') }}
```

* If you set `overwrite_columns`, dbt generates SQL that explicitly lists the columns in both the `INSERT` target and the `SELECT` projection:

  ```sql
  insert overwrite into my_schema.my_table (id, value, event_date)
  select id, value, event_date
  from staging_table
  ```

* If you don't set `overwrite_columns`, dbt currently defaults to `SELECT *`:

  ```sql
  insert overwrite into my_schema.my_table
  select *
  from staging_table
  ```

## Configuring table clustering[​](#configuring-table-clustering "Direct link to Configuring table clustering")

dbt supports [table clustering](https://docs.snowflake.net/manuals/user-guide/tables-clustering-keys.html) on Snowflake. To control clustering for a table or incremental model, use the `cluster_by` config. When this configuration is applied, dbt will do two things:

1. It will implicitly order the table results by the specified `cluster_by` fields.
2. It will add the specified clustering keys to the target table.

By using the specified `cluster_by` fields to order the table, dbt minimizes the amount of work required by Snowflake's automatic clustering functionality. If an incremental model is configured to use table clustering, then dbt will also order the staged dataset before merging it into the destination table. As such, the dbt-managed table should always be in a mostly clustered state.

### Using cluster\_by[​](#using-cluster_by "Direct link to Using cluster_by")

The `cluster_by` config accepts either a string, or a list of strings to use as clustering keys. The following example will create a sessions table that is clustered by the `session_start` column.

models/events/sessions.sql

```sql
{{
  config(
    materialized='table',
    cluster_by=['session_start']
  )
}}

select
  session_id,
  min(event_time) as session_start,
  max(event_time) as session_end,
  count(*) as count_pageviews

from {{ source('snowplow', 'event') }}
group by 1
```

The code above will be compiled to SQL that looks (approximately) like this:

```sql
create or replace table my_database.my_schema.my_table as (

  select * from (
    select
      session_id,
      min(event_time) as session_start,
      max(event_time) as session_end,
      count(*) as count_pageviews

    from {{ source('snowplow', 'event') }}
    group by 1
  )

  -- this order by is added by dbt in order to create the
  -- table in an already-clustered manner.
  order by session_start

);

 alter table my_database.my_schema.my_table cluster by (session_start);
```

### Dynamic table clustering[​](#dynamic-table-clustering "Direct link to Dynamic table clustering")

Starting in dbt Core v1.11, dynamic tables support the `cluster_by` configuration. When set, dbt includes the clustering specification in the `CREATE DYNAMIC TABLE` statement.

For example:

```sql
{{ config(
    materialized='dynamic_table',
    snowflake_warehouse='COMPUTE_WH',
    target_lag='1 minute',
    cluster_by=['session_start', 'user_id']
) }}

select
    session_id,
    user_id,
    min(event_time) as session_start,
    max(event_time) as session_end,
    count(*) as count_pageviews
from {{ source('snowplow', 'event') }}
group by 1, 2
```

This config generates the following SQL when compiled:

```sql
create or replace dynamic table my_database.my_schema.my_table
  target_lag = '1 minute'
  warehouse = COMPUTE_WH
  cluster by (session_start, user_id)
as (
  select
    session_id,
    user_id,
    min(event_time) as session_start,
    max(event_time) as session_end,
    count(*) as count_pageviews
  from source_table
  group by 1, 2
);
```

You can specify clustering for dynamic tables when you create them using `CLUSTER BY` in the `CREATE DYNAMIC TABLE` statement. You don’t need to run a separate `ALTER TABLE` statement.

### Automatic clustering[​](#automatic-clustering "Direct link to Automatic clustering")

Automatic clustering is [enabled by default in Snowflake today](https://docs.snowflake.com/en/user-guide/tables-auto-reclustering.html), no action is needed to make use of it. Though there is an `automatic_clustering` config, it has no effect except for accounts with (deprecated) manual clustering enabled.

If [manual clustering is still enabled for your account](https://docs.snowflake.com/en/user-guide/tables-clustering-manual.html), you can use the `automatic_clustering` config to control whether or not automatic clustering is enabled for dbt models. When `automatic_clustering` is set to `true`, dbt will run an `alter table <table name> resume recluster` query after building the target table.

The `automatic_clustering` config can be specified in the `dbt_project.yml` file, or in a model `config()` block.

dbt\_project.yml

```yaml
models:
  +automatic_clustering: true
```

## Python model configuration[​](#python-model-configuration "Direct link to Python model configuration")

The Snowflake adapter supports Python models. Snowflake uses its own framework, Snowpark, which has many similarities to PySpark.

**Additional setup:** You will need to [acknowledge and accept Snowflake Third Party Terms](https://docs.snowflake.com/en/developer-guide/udf/python/udf-python-packages.html#getting-started) to use Anaconda packages.

**Installing packages:** Snowpark supports several popular packages via Anaconda. Refer to the [complete list](https://repo.anaconda.com/pkgs/snowflake/) for more details. Packages are installed when your model is run. Different models can have different package dependencies. If you use third-party packages, Snowflake recommends using a dedicated virtual warehouse for best performance rather than one with many concurrent users.

**Python version:** To specify a different Python version, use the following configuration:

```python
def model(dbt, session):
    dbt.config(
        materialized = "table",
        python_version="3.11"
    )
```

You can use the `python_version` config to run a Snowpark model with [Python versions](https://docs.snowflake.com/en/developer-guide/snowpark/python/setup) 3.9, 3.10, or 3.11.

**External access integrations and secrets**: To query external APIs within dbt Python models, use Snowflake’s [external access](https://docs.snowflake.com/en/developer-guide/external-network-access/external-network-access-overview) together with [secrets](https://docs.snowflake.com/en/developer-guide/external-network-access/secret-api-reference). Here are some additional configurations you can use:

```python
import pandas
import snowflake.snowpark as snowpark

def model(dbt, session: snowpark.Session):
    dbt.config(
        materialized="table",
        secrets={"secret_variable_name": "test_secret"},
        external_access_integrations=["test_external_access_integration"],
    )
    import _snowflake
    return session.create_dataframe(
        pandas.DataFrame(
            [{"secret_value": _snowflake.get_generic_secret_string('secret_variable_name')}]
        )
    )
```

**Docs:** ["Developer Guide: Snowpark Python"](https://docs.snowflake.com/en/developer-guide/snowpark/python/index.html)

### Third-party Snowflake packages[​](#third-party-snowflake-packages "Direct link to Third-party Snowflake packages")

To use a third-party Snowflake package that isn't available in Snowflake Anaconda, upload your package by following [this example](https://docs.snowflake.com/en/developer-guide/udf/python/udf-python-packages#importing-packages-through-a-snowflake-stage), and then configure the `imports` setting in the dbt Python model to reference to the zip file in your Snowflake staging.

Here’s a complete example configuration using a zip file, including using `imports` in a Python model:

```python

def model(dbt, session):
    # Configure the model
    dbt.config(
        materialized="table",
        imports=["@mystage/mycustompackage.zip"],  # Specify the external package location
    )
    
    # Example data transformation using the imported package
    # (Assuming `some_external_package` has a function we can call)
    data = {
        "name": ["Alice", "Bob", "Charlie"],
        "score": [85, 90, 88]
    }
    df = pd.DataFrame(data)

    # Process data with the external package
    df["adjusted_score"] = df["score"].apply(lambda x: some_external_package.adjust_score(x))
    
    # Return the DataFrame as the model output
    return df
```

For more information on using this configuration, refer to [Snowflake's documentation](https://community.snowflake.com/s/article/how-to-use-other-python-packages-in-snowpark) on uploading and using other python packages in Snowpark not published on Snowflake's Anaconda channel.

## Configuring virtual warehouses[​](#configuring-virtual-warehouses "Direct link to Configuring virtual warehouses")

The default warehouse that dbt uses can be configured in your [Profile](https://docs.getdbt.com/docs/local/profiles.yml.md) for Snowflake connections. To override the warehouse that is used for specific models (or groups of models), use the `snowflake_warehouse` model configuration. This configuration can be used to specify a larger warehouse for certain models in order to control Snowflake costs and project build times.

[Tests](https://docs.getdbt.com/docs/build/data-tests.md) also supports the `snowflake_warehouse` configuration. This can be useful when you want to you run tests on a different Snowflake virtual warehouse than the one used to build models, for example, using a smaller warehouse for lightweight data tests while models run on a larger warehouse.

* Project file
* Property file
* SQL file config

The following example changes the warehouse for a group of models with a config argument in the YAML.

dbt\_project.yml

```yaml
name: my_project
version: 1.0.0

...

models:
  +snowflake_warehouse: "EXTRA_SMALL"    # default Snowflake virtual warehouse for all models in the project.
  my_project:
    clickstream:
      +snowflake_warehouse: "EXTRA_LARGE"    # override the default Snowflake virtual warehouse for all models under the `clickstream` directory.
snapshots:
  +snowflake_warehouse: "EXTRA_LARGE"    # all Snapshot models are configured to use the `EXTRA_LARGE` warehouse.
data_tests:
  +snowflake_warehouse: "EXTRA_SMALL"    # all data tests are configured to use the `EXTRA_SMALL` warehouse.
```

The following example overrides the Snowflake warehouse for a single model and a specific test using a config argument in the property file.

models/my\_model.yml

```yaml
models:
  - name: my_model
    config:
      snowflake_warehouse: "EXTRA_LARGE"    # override the Snowflake virtual warehouse just for this model
    columns:
      - name: id
        data_tests:
          - unique:
              config:
                snowflake_warehouse: "EXTRA_SMALL"    # use a smaller warehouse for this test
```

The following example changes the warehouse for a single model with a config() block in the SQL model.

models/events/sessions.sql

```sql
# override the Snowflake virtual warehouse for just this model
{{
  config(
    materialized='table',
    snowflake_warehouse='EXTRA_LARGE'
  )
}}

with

aggregated_page_events as (

    select
        session_id,
        min(event_time) as session_start,
        max(event_time) as session_end,
        count(*) as count_page_views
    from {{ source('snowplow', 'event') }}
    group by 1

),

index_sessions as (

    select
        *,
        row_number() over (
            partition by session_id
            order by session_start
        ) as page_view_in_session_index
    from aggregated_page_events

)

select * from index_sessions
```

## Copying grants[​](#copying-grants "Direct link to Copying grants")

When the `copy_grants` config is set to `true`, dbt will add the `copy grants` DDL qualifier when rebuilding tables, views, and [dynamic tables](#copy-grants-dynamic-tables) (`dbt-snowflake` v1.11 and later). The default value is `false`.

dbt\_project.yml

```yaml
models:
  +copy_grants: true
```

## Setting row access policies[​](#setting-row-access-policies "Direct link to Setting row access policies")

Configure [row access policies](https://docs.snowflake.com/en/user-guide/security-row-intro) on tables, views, and dynamic tables by using the `row_access_policy` config for models. The policy must already exist in Snowflake before you apply it to the model.

models/\<modelname>.sql

```sql
{{ config(
    row_access_policy = 'my_database.my_schema.my_row_access_policy_name on (id)'
) }}

select ...
```

## Configuring table tags[​](#configuring-table-tags "Direct link to Configuring table tags")

To add tags to tables, views, and dynamic tables, use the `table_tag` config. Note, the tag must already exist in Snowflake before you apply it.

models/\<modelname>.sql

```sql
{{ config(
    table_tag = "my_tag_name = 'my_tag_value'"
) }}

select ...
```

## Secure views[​](#secure-views "Direct link to Secure views")

To create a Snowflake [secure view](https://docs.snowflake.net/manuals/user-guide/views-secure.html), use the `secure` config for view models. Secure views can be used to limit access to sensitive data. Note: secure views may incur a performance penalty, so you should only use them if you need them.

The following example configures the models in the `sensitive/` folder to be configured as secure views.

dbt\_project.yml

```yaml
name: my_project
version: 1.0.0

models:
  my_project:
    sensitive:
      +materialized: view
      +secure: true
```

## Source freshness known limitation[​](#source-freshness-known-limitation "Direct link to Source freshness known limitation")

Snowflake calculates source freshness using information from the `LAST_ALTERED` column, meaning it relies on a field updated whenever any object undergoes modification, not only data updates. No action must be taken, but analytics teams should note this caveat.

Per the [Snowflake documentation](https://docs.snowflake.com/en/sql-reference/info-schema/tables#usage-notes):

> The `LAST_ALTERED` column is updated when the following operations are performed on an object:
>
> * DDL operations.
> * DML operations (for tables only).
> * Background maintenance operations on metadata performed by Snowflake.

## Pagination for object results[​](#pagination-for-object-results "Direct link to Pagination for object results")

By default, when dbt encounters a schema with up to 100,000 objects, it will paginate the results from `show objects` at 10,000 per page for up to 10 pages.

Environments with more than 100,000 objects in a schema can customize the number of results per page and the page limit using the following [flags](https://docs.getdbt.com/reference/global-configs/about-global-configs.md) in the `dbt_project.yml`:

* `list_relations_per_page` — The number of relations on each page (Max 10k as this is the most Snowflake allows).
* `list_relations_page_limit` — The maximum number of pages to include in the results.

For example, if you wanted to include 10,000 objects per page and include up to 100 pages (1 million objects), configure the flags as follows:

```yml

flags:
  list_relations_per_page: 10000
  list_relations_page_limit: 100
```

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
