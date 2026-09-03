# dbt state explain

(Applies to dbt v2.0 and later) `dbt state explain` is a CLI command available when you enable [dbt State](../../docs/deploy/dbt-state-about.md). After a job finishes, run this command to see why dbt State made each decision for every node and whether it executed, skipped, or cloned the node. Use it to audit State behavior, debug unexpected skips, or confirm that freshness and query checks work as expected.

(Applies to dbt v2.0 and later)

```bash
dbt state explain
```

The output shows all nodes from the last run, with a summary of the State decision for each:

```shell
SKIP_EXECUTION model.jaffle_shop.customers - model was a no-op because its query is up to date and its upstream data is within freshness tolerance
READY_TO_EXECUTE model.jaffle_shop.orders - model was executed because the view definition is newer than the cached execution
READY_TO_EXECUTE test.jaffle_shop.not_null_customers_customer_id - data test was executed because it has no prior execution or its query changed
UNKNOWN unit_test.jaffle_shop.orders.test_order_items_compute_to_bools_correctly - dbt State explain details unavailable
```

If you use the dbt platform, the same information is available without running a command — go to the [**Explain** tab](../../docs/deploy/dbt-state-interface.md#explain-tab) on the job run details page to see the full decision breakdown for each node.

## Specifying a log file

By default, (Applies to dbt v2.0 and later) `dbt state explain` reads from the most recent execution. To analyze a previous run, you can use `--log-file` (or `-l`) to specify a state file from the `logs/state/` directory:

(Applies to dbt v2.0 and later)

```bash
dbt state explain --log-file 'logs/state/responses_2026_08_25_11_00_15_667.jsonl'
```

## Using verbose mode

Use the `--verbose` flag to see the full step-by-step analysis for each node and add a run configuration summary. Use `-s` to filter the output to a specific node.

(Applies to dbt v2.0 and later)

```bash
dbt state explain --verbose -s my_node_name
```

In dbt Core 2.0, `--verbose` adds a run configuration summary at the top and shows the full step-by-step analysis for each node.

```shell
Run configuration:
  - started at: 2026-08-17T12:24:15.822733+00:00
  - profile: my_profile
  - target: dev
  - defer to target: prod
  - freshness tolerance: 2700 seconds
  - tolerate nondeterminism: true
  - clone incremental in dev: IF_TABLE_MISSING
  - metadata cache TTL: 0 seconds
  - select: fqn:my_node_name
SKIP_EXECUTION model.jaffle_shop.customers - model was a no-op because its query is up to date and its upstream data is within freshness tolerance
  - table analysis ("ANALYTICS"."MY_SCHEMA"."CUSTOMERS")
    - the model table exists already [SUCCESS, TARGET_TABLE_EXISTS]
  - query analysis
    - the model query has not changed [SUCCESS, NODE_QUERY_UNCHANGED]
    - upstream model queries have not changed [SUCCESS]
  - data freshness analysis
    - upstream dependencies
      - "ANALYTICS"."MY_SCHEMA_RAW"."RAW_CUSTOMERS" [FRESH]
        - no updates since "ANALYTICS"."MY_SCHEMA"."CUSTOMERS" last executed
        - last updated: 5 days ago
    - upstream data is up to date [SUCCESS]
```

The `--verbose` flag produces a decision breakdown that may include the following analyses, depending on the node type:

* **Table analysis**: whether the target table already exists in the schema
* **Query analysis**: whether the model query or its upstream queries have changed
* **Data freshness analysis**: whether upstream data is fresh or within the configured [`lag_tolerance`](../resource-configs/lag-tolerance.md)

## Related docs

* [About dbt State](../../docs/deploy/dbt-state-about.md)
* [Monitor dbt State activity](../../docs/deploy/dbt-state-interface.md)
* [dbt State configs](../resource-configs/dbt-state-configs.md)
