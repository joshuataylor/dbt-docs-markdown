# dbt state explain

(Applies to dbt v1.99 and earlier) `dbt-state explain` is a CLI command available when you enable [dbt State](../../docs/deploy/dbt-state-about.md). After a job finishes, run this command to see why dbt State made each decision for every node and whether it executed, skipped, or cloned the node. Use it to audit State behavior, debug unexpected skips, or confirm that freshness and query checks work as expected.

(Applies to dbt v1.99 and earlier)

```bash
dbt-state explain
```

The output shows all nodes from the last run with a summary of the State decision for each:

```shell
Last run: 2 minutes ago

customers
└── [No-op] model was a no-op because its query is up to date and its upstream data is within freshness tolerance

locations
└── [Execute] model was executed because either its query didn't match or its upstream data is out of date

not_null_stg_locations_location_id
└── [Execute] data test was executed because it has no prior execution or its query changed
```

If you use the dbt platform, the same information is available without running a command — go to the [**Explain** tab](../../docs/deploy/dbt-state-interface.md#explain-tab) on the job run details page to see the full decision breakdown for each node.

## Specifying a log file

By default, (Applies to dbt v1.99 and earlier) `dbt-state explain` reads from the most recent execution. To analyze a previous run, you can use `--log-file` (or `-l`) to specify a state file from the `logs/state/` directory:

(Applies to dbt v1.99 and earlier)

```bash
dbt-state explain --log-file 'logs/state/responses_2026_08_25_11_00_15_667.jsonl'
```

## Using verbose mode

Use the `--verbose` flag to see the full step-by-step analysis for each node and add a run configuration summary. Use `-s` to filter the output to a specific node.

(Applies to dbt v1.99 and earlier)

```bash
dbt-state explain --verbose -s my_node_name
```

The verbose output shows the full step-by-step analysis and adds a run configuration summary:

```shell
Last run: 2026-08-19 20:19:00 PST (10 minutes ago)

  Run configuration:
  ├── dbt State:
  │   ├── defer to target: prod
  │   ├── freshness tolerance: 45 minutes (2700 seconds)
  │   ├── tolerate non-determinism: True
  │   ├── clone incremental in dev: IF_TABLE_MISSING
  │   └── metadata cache ttl: infinite (cache never expires)
  └── dbt:
      ├── profile: jaffle_shop
      └── target: dev

  customers
  ├── table analysis ("ANALYTICS"."DBT_SCHEMA"."CUSTOMERS")
  │   └── ✓ the model table exists already in 'dev'
  ├── query analysis
  │   ├── ✓ the model query has not changed
  │   └── ✓ upstream model queries have not changed
  └── data freshness analysis
      ├── upstream dependencies
      │   ├── [fresh] "ANALYTICS"."RAW"."RAW_CUSTOMERS"
      │   │   ├── no updates since "ANALYTICS"."DBT_SCHEMA"."CUSTOMERS" last
      │   │   │   executed
      │   │   └── last updated: a month ago, 2026-07-31 13:22:30 UTC
      │   └── [within tolerance] "ANALYTICS"."DBT_SCHEMA"."ORDERS"
      │       ├── updated a moment after "ANALYTICS"."DBT_SCHEMA"."CUSTOMERS" last
      │       │   executed (tolerance: 45 minutes)
      │       └── last updated: 39 seconds ago, 2026-08-19 12:18:40 UTC
      └── ✓ upstream data is outdated (within tolerance)
  decision: [No-op] model was a no-op because its query is up to date and its upstream data is within freshness tolerance
```

The `--verbose` flag produces a decision breakdown that may include the following analyses, depending on the node type:

* **Table analysis**: whether the target table already exists in the schema
* **Query analysis**: whether the model query or its upstream queries have changed
* **Data freshness analysis**: whether upstream data is fresh or within the configured [`lag_tolerance`](../resource-configs/lag-tolerance.md)

## Related docs

* [About dbt State](../../docs/deploy/dbt-state-about.md)
* [Monitor dbt State activity](../../docs/deploy/dbt-state-interface.md)
* [dbt State configs](../resource-configs/dbt-state-configs.md)
