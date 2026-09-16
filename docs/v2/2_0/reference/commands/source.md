# About dbt source command

(Applies to dbt v2.0 and later)

The `dbt source` command provides one legacy subcommand, `dbt source freshness`, which evaluates sources only.

In v2, use the top-level [`dbt freshness`](./freshness.md) command to evaluate both sources and models against their configured [freshness thresholds](../resource-configs/freshness.md) in a single command.

### Freshness

(Applies to dbt v2.0 and later)

If your dbt project is [configured with sources](../../docs/build/sources.md), dbt determines the "freshness" of each source table from the `freshness` config specified for that source. If a table is stale, dbt reports a warning or error accordingly and exits with a nonzero exit code.

To evaluate source freshness, run [`dbt freshness`](./freshness.md).

### Configure freshness

The example below shows how to configure source freshness in dbt. Refer to [Declaring source freshness](../../docs/build/sources.md#declaring-source-freshness) for more information.

models/\<filename>.yml

```yaml


sources:
  - name: jaffle_shop
    database: raw
    config:
      freshness: # changed to config in v1.9
        warn_after: {count: 12, period: hour}
        error_after: {count: 24, period: hour}

      loaded_at_field: _etl_loaded_at # changed to config in v1.10

    tables:
      - name: customers

      - name: orders
        config:
          freshness: 
            warn_after: {count: 6, period: hour}
            error_after: {count: 12, period: hour}
            filter: datediff('day', _etl_loaded_at, current_timestamp) < 2

      - name: product_skus
        config:
          freshness: null 
          
```

This helps to monitor the data pipeline health.

You can also configure source freshness in the **Execution settings** section in your dbt platform job **Settings** page. For more information, refer to [Enabling source freshness checks](../../docs/deploy/source-freshness.md#enabling-source-freshness-checks).

(Applies to dbt v2.0 and later)

### Evaluate specific sources

By default, `dbt freshness` evaluates every source and model in your project that has a `freshness` config. To evaluate a subset of your sources, use the `--select` flag.

```bash
# Evaluate freshness for all Snowplow tables:
$ dbt freshness --select "source:snowplow"

# Evaluate freshness for a particular source table:
$ dbt freshness --select "source:snowplow.event"
```

(Applies to dbt v2.0 and later)

### Freshness output

When `dbt freshness` completes, dbt writes results for the evaluated sources and models to `target/freshness.json`. Each entry includes a `resource_type` field identifying whether the resource is a source or a model. For the full schema, refer to [`freshness.json`](../artifacts/freshness-json.md).

Whenever sources are included in the run, dbt also writes `target/sources.json` for backward compatibility. It contains sources only, with no `resource_type` field. For the full schema, refer to [`sources.json`](../artifacts/sources-json.md).

(Applies to dbt v2.0 and later)

### Using freshness results

Freshness results help you understand whether a source or model is in a delayed state, and how freshness trends over time.

You can evaluate freshness manually at any time with [`dbt freshness`](./freshness.md). We also recommend running it on a schedule and storing the results at regular intervals. These longitudinal results make it possible to be alerted when source data freshness SLAs are violated, and to understand the trend of freshness over time.

dbt makes it easy to run freshness checks on a schedule, and provides a dashboard out of the box indicating the state of freshness for the sources defined in your project. For more information, refer to [source data freshness](../../docs/build/sources.md#source-data-freshness).
