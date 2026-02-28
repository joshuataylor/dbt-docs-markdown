# Near real-time data in dbt

By design, dbt is batch-oriented with jobs having a defined start and end time. But did you know that you can also use dbt to get near real-time data by combining your data warehouse's continuous ingestion with frequent dbt transformations?

This guide covers multiple patterns for achieving near real-time data freshness with dbt:

1. [Incremental patterns](https://docs.getdbt.com/best-practices/how-we-handle-real-time-data/2-incremental-patterns.md) — `merge` strategies, Change Data Capture (CDC), and microbatch processing
2. [Warehouse-native features](https://docs.getdbt.com/best-practices/how-we-handle-real-time-data/3-warehouse-native-features.md) — When to use dynamic tables and materialized views
3. [Lambda views pattern](https://docs.getdbt.com/best-practices/how-we-handle-real-time-data/4-lambda-views.md) — Combining batch and real-time data in a single view
4. [Views-only pattern](https://docs.getdbt.com/best-practices/how-we-handle-real-time-data/5-views-only-pattern.md) - Maximum freshness for lightweight transformations
5. [Operational considerations](https://docs.getdbt.com/best-practices/how-we-handle-real-time-data/6-operational-considerations.md) — Challenges, risks, and cost management

Each pattern includes practical code examples, use cases, and tradeoffs to help you choose the right approach.

Anyone can use this guide, but it's primarily for data engineers and architects who want to achieve near real-time data freshness with dbt.

## Where does dbt fit?[​](#where-does-dbt-fit "Direct link to Where does dbt fit?")

There are two main ways to use dbt to get near real-time data:

* For near real-time (5 - 15 minutes) — dbt excels at this and is well-suited for most operational dashboards.
* For true real-time (sub-second) — This requires dedicated streaming databases (ClickHouse, Materialize, Rockset, and so on) in front of or alongside dbt; dbt still owns “analytic” tables and history but not the ultra‑low‑latency read path.

## How dbt achieves near real-time data[​](#how-dbt-achieves-near-real-time-data "Direct link to How dbt achieves near real-time data")

To achieve real-time data with dbt, we recommend using a two-layer architecture:

#### Ingestion layer[​](#ingestion-layer "Direct link to Ingestion layer")

Continuous data landing using your data warehouse's streaming ingestion features.

Streaming ingestion features such as [streaming tables](https://docs.databricks.com/en/sql/load-data-streaming-table.html), [Snowpipe](https://docs.snowflake.com/en/user-guide/snowpipe-streaming/data-load-snowpipe-streaming-overview), or [Storage Write API](https://docs.cloud.google.com/bigquery/docs/write-api-streaming) work well for this. To find streaming ingestion features for your warehouse, refer to the [additional resources](https://docs.getdbt.com/best-practices/how-we-handle-real-time-data/3-warehouse-native-features.md#resources-by-warehouse) section.

#### dbt transformation layer[​](#dbt-transformation-layer "Direct link to dbt transformation layer")

Run dbt every few minutes to transform the data, and use materialized views or dynamic tables for the lowest-latency reporting.

Specific transformation approaches include:

* [Incremental models](https://docs.getdbt.com/docs/build/incremental-models-overview.md) with merge or append strategies
* [Microbatch incremental strategy](https://docs.getdbt.com/docs/build/incremental-microbatch.md) for large time-series tables
* Jobs scheduled very frequently (like every 5 minutes)
* [Dynamic tables](https://docs.getdbt.com/reference/resource-configs/snowflake-configs.md#dynamic-tables) or [materialized views](https://docs.getdbt.com/docs/build/materializations.md#materialized-view) with short refresh intervals

## Key recommendations[​](#key-recommendations "Direct link to Key recommendations")

The following are some key recommendations to help you achieve near real-time data freshness with dbt:

* Ingest data continuously: Use your warehouse's native streaming or micro-batch ingestion to land raw data as soon as it arrives.
* Transform with dbt on a frequent schedule: Schedule dbt jobs to run as often as your business needs allow (for example, every 1–15 minutes). Balance freshness with cost and resource constraints.
* Materialized views and dynamic tables: For the lowest-latency reporting, use materialized views or dynamic tables. These can be refreshed as frequently as every minute.
* Incremental models and microbatching: Use dbt's incremental models to process only new or changed data, keeping transformations efficient and scalable.
* Decouple ingestion from transformation: Keep data acquisition and transformation flows separate. This allows you to optimize each independently.
* Monitor and test data freshness: Implement data quality checks and freshness monitoring to ensure your near real-time pipelines deliver accurate, up-to-date results.
* Cost and complexity considerations: Running dbt jobs more frequently drives up compute costs and operational complexity. Always weigh the business value against these trade-offs.

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
